<properties 
    pageTitle="Met behulp van lineaire regressie in Machine Learning | Microsoft Azure" 
    description="Een vergelijking van modellen voor lineaire regressie in Excel en in Azure Machine Learning Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Met behulp van lineaire regressie in Azure Machine Learning

> *Kate Baroni* en *Ben Boatman* zijn enterprise solution architects in van Microsoft gegevens Insights Center van topkwaliteit. In dit artikel beschrijven ze hun ervaring een bestaande regressie-analyse pakket migreren naar een cloud-oplossing met Azure Machine Learning.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Doelstelling

Onze project gestart met twee doelen:  

1. Voorspellende analytics gebruiken voor het verbeteren van de nauwkeurigheid van de maandelijkse inkomsten projecties van onze organisatie  
2. Azure ML gebruikt om te bevestigen, optimaliseren, snelheid, vergroten en de schaal van onze resultaten.  

Onze organisatie doorloopt een maandelijkse omzet voorspellen proces net als veel bedrijven. Ons kleine team van analisten is ter ondersteuning van het proces en prognose nauwkeurigheid verbeteren met behulp van Machine Learning onderbroken.  Het team besteed aan enkele maanden voor het verzamelen van gegevens uit meerdere bronnen en de gegevenskenmerken door middel van statistische analyse identificeert belangrijke kenmerken relevant voor verkoopprognoses services wordt uitgevoerd.  Volgende stap is om te beginnen met het maken van prototypen statistische regressie-modellen van de gegevens in Excel.  We hadden een regressiemodel Excel dat feit is dat het huidige veld en de financiële prognoses processen binnen een paar weken. Dit is het resultaat van de voorspelling basislijn geworden.  


Vervolgens werd de volgende stap naar onze voorspellende analytics over te gaan tot Azure ML om erachter te komen hoe Azure ML op voorspellende prestatie kan verbeteren.


## <a name="achieving-predictive-performance-parity"></a>Te bereiken pariteit voorspellende prestatie

Onze eerste prioriteit is te bereiken pariteit tussen Azure ML en Excel regressie-modellen.  De exacte dezelfde gegevens en de splitsing voor training en testen van de gegevens die we wilden bereiken pariteit tussen Excel en Azure ML voorspellende prestatie gegeven.   In eerste instantie is mislukt. Het model Azure ML ver-loop van het Excel-objectmodel.   De fout is veroorzaakt door een gebrek aan begrip van de instelling van het basis gereedschap in Azure ML. Na een synchronisatie met het productteam Azure ML, we een beter begrip van de instelling voor onze gegevenssets base opgedaan en pariteit tussen de twee modellen bereikt.  

### <a name="create-regression-model-in-excel"></a>Regressiemodel maken in Excel
Onze regressie Excel gebruikt de standaard lineaire regressiemodel gevonden in de Analysis ToolPak voor Excel. 

We berekend *gemiddelde Absolute % fout* en deze gebruikt als het meten van de prestaties van het model.  Het duurde 3 maanden om te komen tot een werkend model met Excel.  We veel van de training in het experiment Azure ML die uiteindelijk zeer nuttig om vereisten is gebracht.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vergelijkbaar experiment in Azure Machine Learning maken  
We deze stappen uit om ons experiment in Azure ML gevolgd:  

1.  De dataset geüpload als een csv-bestand op Azure ML (zeer kleine bestand)
2.  Een nieuw experiment gemaakt en de [Kolommen selecteren in de Dataset] [ select-columns] module te selecteren van de functies van dezelfde gegevens gebruikt in Excel   
3.  De [Gesplitste gegevens] gebruikt[ split] module (met het *Expressie relatieve* modus) verdelen de gegevens in exact dezelfde trein sets zoals is gebeurd in Excel  
4.  Geëxperimenteerd met de [Lineaire regressie] [ linear-regression] module (alleen standaardopties), gedocumenteerd en de resultaten van onze regressiemodel Excel vergeleken

### <a name="review-initial-results"></a>De eerste resultaten bekijken
Op het eerste, het Excel-objectmodel duidelijk ver-loop van het model Azure ML:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Prestaties|   |  |
|<ul style="list-style-type: none;"><li>Aangepast R-kwadraat</li></ul>| 0.96 |N.V.T.|
|<ul style="list-style-type: none;"><li>Coëfficiënt <br />Bepaling</li></ul>|N.V.T.|   0.78<br />(lage nauwkeurigheid)|
|Gemiddelde Absolute fout |  $9. 5M|  M $ 19.4|
|Gemiddelde Absolute fout (%)|   6.03%|  12.2%

Toen we proces en de resultaten door ontwikkelaars en wetenschappers van de gegevens in het team Azure ML uitvoerde, verstrekt zij snel enkele nuttige tips.  

* Als u de [Lineaire regressie] gebruikt[ linear-regression] module in Azure ML, twee methoden zijn beschikbaar:
    *  Online verloop afkomst: Mogelijk beter geschikt voor grotere problemen
    *  Gewone kleinste kwadraten: Dit is de methode denken wanneer zij lineaire regressie horen. Voor kleine datasets zijn gewone kleinste kwadraten een meer optimale keuze.
*  U kunt u de parameter L2 regularisatie gewicht om prestaties te verbeteren. Deze is ingesteld op 0,001 standaard en voor onze kleine gegevensset, stellen we op 0,005 om prestaties te verbeteren.    

### <a name="mystery-solved"></a>Mysterie opgelost!
Wanneer we de aanbevelingen toegepast, bereikt we het dezelfde basislijn in Azure ML als met Excel:   

|| Excel|Azure ML (eerste)|Azure ML met kleinste kwadraten|
|---|:---:|:---:|:---:|
|Waarde met label  |Werkelijke waarden (numeriek)|dezelfde|dezelfde|
|Cursist  |Excel-gegevens > analyse regressie ->|Lineaire regressie.|Lineaire regressie|
|Opties voor cursist|N.V.T.|Standaardwaarden|gewone kleinste kwadraten<br />L2 0,005 =|
|Gegevensverzameling|26 rijen, 3 onderdelen, 1 label.   Alle numerieke.|dezelfde|dezelfde|
|Splitsen: trein|Excel getraind voor de eerste 18 rijen zijn getest op de laatste 8 rijen.|dezelfde|dezelfde|
|Splitsen: Test|Regressie-formule toegepast op de laatste 8 rijen in Excel|dezelfde|dezelfde|
|**Prestaties**||||
|Aangepast R-kwadraat|0.96|N.V.T.||
|Determinatiecoëfficiënt|N.V.T.|0.78|0.952049|
|Gemiddelde Absolute fout |$9. 5M|M $ 19.4|$9. 5M|
|Gemiddelde Absolute fout (%)|<span style="background-color: 00FF00;">6.03%</span>|12.2%|<span style="background-color: 00FF00;">6.03%</span>|

Bovendien de coëfficiënten Excel ten opzichte van zowel de gewichten van de functie in het Azure opgeleide model:

||Coëfficiënten voor Excel|Azure functie gewichten|
|---|:---:|:---:|
|Snijpunt/Bias|19470209.88|19328500|
|Een functie|0.832653063|0.834156|
|Onderdeel B|11071967.08|11007300|
|Onderdeel C|25383318.09|25140800|

## <a name="next-steps"></a>Volgende stappen

We wilden Azure ML webservice binnen Excel verbruiken.  Onze business analisten zijn gebaseerd op Excel en we Roep de webservice Azure ML met een rij van Excel-gegevens en het de voorspelde waarde terug naar Excel nodig.   

We ook wilden onze model optimaliseren met behulp van de opties en de algoritmen die beschikbaar zijn in Azure ML.

### <a name="integration-with-excel"></a>Integratie met Excel
Onze oplossing was het mogelijk onze regressiemodel Azure ML maken door het maken van een webservice uit het model zijn opgeleid.  Binnen enkele minuten de webservice gemaakt en kan noemen we deze rechtstreeks vanuit Excel een waarde van de verwachte opbrengst.    

De sectie *Web Services Dashboard* bevat een downloadbare Excel-werkmap.  De werkmap wordt met de web service API en schema ingesloten informatie vooraf opgemaakte geleverd.   Wanneer u op een *Excel-werkmap downloaden*klikt, wordt het geopend en kunt u deze opslaan op uw lokale computer.    

![][1]
 
Kopieer de vooraf gedefinieerde parameters met de geopende werkmap in het blauwe gedeelte van de Parameter zoals hieronder wordt weergegeven.  Als de parameters zijn opgegeven, uitgevoerd naar de webservice AzureML wordt en de voorspelde gescoord weergegeven labels in het groene gedeelte van de voorspelde waarden.  De werkmap wordt voortgezet voor het maken van prognoses voor de parameters op basis van een opgeleide model voor alle rij-items onder Parameters ingevoerd.   Zie [een webservice Azure Machine Learning uit Excel gebruiken](machine-learning-consuming-from-excel.md)voor meer informatie over het gebruik van deze functie. 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Verdere experimenten en optimaliseren
Nu dat we onze Excel-model met een basislijn hebt gehad, verplaatst we vooruit naar onze Azure ML lineaire regressiemodel optimaliseren.  Hiernaast is de module [functies selecteren op basis van een Filter] [ filter-based-feature-selection] voor het verbeteren van onze selectie van initiële gegevens elementen en het heeft ons geholpen een prestaties verbeterd van 4,6% gemiddelde Absolute fout.   We gebruiken deze functie die kan worden opgeslagen ons weken in de gegevenskenmerken te vinden van de juiste set van functies voor modellering doorlopen voor toekomstige projecten.  

We willen naast zijn andere algoritmen zoals [Bayesiaanse] [ bayesian-linear-regression] of [Beslissingsstructuren versterkt] [ boosted-decision-tree-regression] bij ons experiment prestaties te vergelijken.    

Als u experimenteren met regressie wilt, is een goede dataset om te proberen de dataset monster regressie van energie-efficiëntie, die veel numerieke kenmerken. De dataset wordt geleverd als onderdeel van de datasets monster in ML Studio.  Een aantal modules leren kunt u voorspellen laden verwarming of koeling laden.  In onderstaande tabel wordt dat een vergelijking van de prestaties van verschillende regressie aanleren ten opzichte van de energie-efficiëntie dataset voorspellen voor de variabele doel koeling laden: 

|Model|Gemiddelde Absolute fout|Root Mean Squared fout|Relatieve, Absolute fout|Relatieve kwadraat fout|Determinatiecoëfficiënt
|---|---|---|---|---|---
|Beslissingsstructuur gestimuleerd|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineaire regressie (kleurovergang afkomst)|2.035693|2.98006|0.233414|0.094881|0.905119
|Neural Network regressie|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineaire regressie (gewone kleinste kwadraten)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Belangrijke Takeaways 

Hebt u geleerd veel door uit de actieve Excel-regressie en Azure Machine Learning proeven gelijktijdig. De baseline model maken in Excel en vergelijken met modellen met Azure ML [Lineaire regressie] [ linear-regression] ertoe bijgedragen ons informatie over Azure ML en hebben we ontdekt mogelijkheden om gegevens selecteren en het model prestaties te verbeteren.         

Wij vinden ook dat het wenselijk is te gebruiken [functies selecteren op basis van een Filter] [ filter-based-feature-selection] voorspelling van toekomstige projecten te versnellen.  Door de Functieselectie op uw gegevens toepast, kunt u een verbeterde model in Azure ML met betere algehele prestaties. 

De mogelijkheid om over te brengen de anticiperende analytische prognose van Azure ML naar Excel systeemkritisch kunnen een aanzienlijke toename in de mogelijkheid te bieden met succes resultaten een brede zakelijke publiek.     


## <a name="resources"></a>Bronnen
Sommige bronnen worden voor helpt die u met regressie werken vermeld:  

* Regressie in Excel.  Als u nooit regressie in Excel hebt geprobeerd, deze zelfstudie kunt u gemakkelijk: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressie vs prognoses.  Tyler Chessman schreef een blog-artikel waarin wordt uitgelegd hoe reeksen prognoses in Excel met een goede beginner beschrijving van lineaire regressie uitvoeren op tijden. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Gewone kleinste kwadraten lineaire regressie: Fouten, problemen en valkuilen.  Voor een inleiding en een bespreking van regressie: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
