<properties
    pageTitle="Analyse van de klant te produceren met behulp van Machine Learning | Microsoft Azure"
    description="Case-study van de ontwikkeling van een geïntegreerd model voor het analyseren en scoren lospeuteren van klant"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analyseren van de klant te produceren met behulp van Azure Machine Learning

##<a name="overview"></a>Overzicht
Dit onderwerp bevat een referentie-implementatie van een klant lospeuteren analyse project is gebouwd met behulp van Azure Machine Learning Studio. Bijbehorende algemene modellen voor de zuinigste oplossing van het probleem van de industriële klant lospeuteren besproken. We ook de nauwkeurigheid van de modellen die zijn gemaakt met de computer leren meten en we beoordelen aanwijzingen voor verdere ontwikkeling.  

### <a name="acknowledgements"></a>Bevestigingen

Dit experiment is ontwikkeld en getest door Serge Berger, Principal gegevens wetenschappelijk onderzoeker bij Microsoft en Roger Barga, voorheen Product Manager voor Microsoft Azure Machine Learning. Het documentatieteam van Azure dank erkent hun deskundigheid en ze Bedankt voor het delen van deze white paper.

>[AZURE.NOTE] De gegevens voor dit experiment is niet openbaar. Zie voor een voorbeeld van het bouwen van een model van de machine learning voor lospeuteren analyse: [Telco produceren model sjabloon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com/)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Het probleem van de klant lospeuteren
Bedrijven in de markt van de consument en in alle sectoren van de onderneming moeten omgaan met lospeuteren. Soms lospeuteren excessief is en beleidsbeslissingen beïnvloedt. De traditionele oplossing is te voorspellen investeringsneiging van de hoge churners en hun behoeften via een service concierge, marketingcampagnes, of door toepassing van speciale dispensatie. Deze benaderingen kunnen verschillen van de bedrijfstak van de bedrijfstak en zelfs van een bepaalde consument cluster binnen een bedrijfstak (bijvoorbeeld telecommunicatie).

De gemeenschappelijke factor is dat bedrijven moeten deze inhouding klant bijzondere inspanningen te minimaliseren. Een natuurlijke methodologie zou dus aan de score van elke klant met de kans op lospeuteren en het adres van de top N prijzen. De belangrijkste klanten mogelijk de meest winstgevende prijzen; in meer geavanceerde scenario's, een winst-functie gebruikt tijdens de selectie van kandidaten voor speciale dispensatie. Deze overwegingen zijn echter slechts een deel van de holistische strategie voor het omgaan met lospeuteren. Bedrijven hebben ook rekening account risico (en bijbehorende risicotolerantie), het niveau en de kosten van de interventie en aannemelijke klant Segmentatie.  

##<a name="industry-outlook-and-approaches"></a>De bedrijfstak van outlook en benaderingen
Geavanceerde verwerking van lospeuteren is een teken van een volwassen branche. Het klassieke voorbeeld is de telecommunicatie-industrie waar abonnees bekend is vaak één provider overschakelt naar een andere. Deze vrijwillige lospeuteren is van primair belang. Bovendien hebben providers verzameld aanzienlijke kennis over *stuurprogramma's te produceren*, zijn de factoren die klanten station om over te schakelen.

Keuze handset of apparaat is bijvoorbeeld een bekende stuurprogramma van lospeuteren in de zakelijke mobiele telefoon. Als gevolg hiervan is een populaire beleid subsidize de prijs van een telefoonhoorn voor nieuwe abonnees en een volledige prijs aan bestaande klanten voor een upgrade in rekening brengen. In het verleden, heeft dit beleid geleid tot de klanten van een provider hopping naar de andere om een nieuwe korting, die op zijn beurt heeft gevraagd of u providers hun strategieën te verfijnen.

Hoge volatiliteit in hoorn aanbiedingen is een factor die zeer snel, wordt ongeldig modellen die zijn gebaseerd op de huidige modellen van de hoorn van lospeuteren. Mobiele telefoons zijn bovendien niet alleen telecommunicatie-apparatuur; ze zijn ook mode instructies (Houd rekening met de iPhone) en deze sociale variabelen worden buiten het bereik van normale telecommunicatie gegevenssets.

Het resultaat voor het model is dat u een goed beleid kan niet ontwerpen simpelweg doordat bekende redenen voor lospeuteren. In feite is een strategie continue modellering, met inbegrip van de klassieke modellen die bestaan variabelen (zoals beslissingsstructuren kwantificeren) **verplicht**.

Met grote gegevenssets in hun klanten, uitvoert organisaties big data analytics (met name lospeuteren detectie op basis van big data) als het probleem op een effectieve manier. U vindt meer informatie over de aanpak van het probleem van het lospeuteren van de aanbevelingen in de sectie ETL big data.  

##<a name="methodology-to-model-customer-churn"></a>De methodologie van de klant lospeuteren van model
Een algemene proces het oplossen van problemen oplossen lospeuteren van de klant wordt afgebeeld in de cijfers 1-3:  

1.  Een risicomodel kunt u overwegen hoe acties invloed op waarschijnlijkheid en risico's.
2.  Een interventie-model kunt u rekening houden met het niveau van de interventie kan de invloed van de kans op lospeuteren en het bedrag van de klant verlooptijd (CLV).
3.  Deze analyse gepaard met een kwalitatieve analyse die een proactieve marketingcampagne die gericht is op klantsegmenten te leveren van de optimale aanbieding is geëscaleerd.  

![][1]

Deze vooruit ogende benadering is de beste manier om het lospeuteren behandelen, maar wordt geleverd met complexiteit: we hebben voor het ontwikkelen van een archetype met meerdere modellen en trace afhankelijkheden tussen de modellen. De interactie tussen modellen kan worden ingekapseld, zoals in het volgende diagram:  

![][2]

*Figuur 4: Unified archetype met meerdere modellen*  

Interactie tussen de modellen is essentieel als we een holistische aanpak bij de inhoudingen klant afgeleverd. Elk model vermindert noodzakelijkerwijs na verloop van tijd; de architectuur is daarom een impliciete lus (vergelijkbaar met het instellen van de standaard de SCHERPE DM datamining, [***3***] archetype).  

De totale cyclus van risico-besluit-marketing segmentatie/ontbinding is nog steeds een algemene structuur, van toepassing op veel zakelijke problemen is. Lospeuteren-analyse is gewoon een sterke vertegenwoordiger van deze groep van problemen omdat deze vertoont de kenmerken van een complexe zakelijke probleem dat een vereenvoudigde voorspellende oplossing is niet toegestaan. De sociale aspecten van de moderne aanpak te produceren met name niet in de aanpak van zijn gemarkeerd, maar de sociale aspecten zijn ingekapseld in het archetype modellering, zoals in een model.  

Hier een interessante toevoeging is big data analytics. Hedendaagse telecommunicatie en detailhandel bedrijven verzamelen van uitvoerige gegevens over hun klanten, en we gemakkelijk kunt voorzien dat de noodzaak voor verbindingen met meerdere modellen een algemene trend, opkomende trends zoals het Internet van dingen en alomtegenwoordige-apparaten, waardoor het bedrijf te gebruiken op meerdere lagen smart-oplossingen gegeven zal worden.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Uitvoering van het archetype modellering in Machine Learning Studio
Gelet op de zojuist beschreven probleem, wat is de beste manier voor het implementeren van een geïntegreerde modellen en scoren aanpak? In dit gedeelte wordt gedemonstreerd hoe we dit doen met behulp van Azure Machine Learning Studio.  

De aanpak met meerdere modellen is onmisbaar bij het ontwerpen van een globale archetype voor lospeuteren. Zelfs de score (voorspellende) deel uit van de aanpak moet worden met meerdere modellen.  

Het volgende diagram ziet het prototype die we hebben gemaakt, die in dienst zijn vier score algoritmen in de Machine Learning Studio te voorspellen lospeuteren. De reden voor het gebruik van een aanpak met meerdere modellen is niet alleen voor het maken van een ensemble classificatie om nauwkeuriger, maar ook beschermen tegen overdreven montage en te verbeteren uitgebreide functies selecteren.  

![][3]

*Figuur 5: Prototype van een lospeuteren benadering van modellering*  

De volgende secties bevatten meer informatie over het prototype model dat we geïmplementeerd met behulp van Machine Learning Studio scoren.  

###<a name="data-selection-and-preparation"></a>Gegevens selecteren en voorbereiden
De gegevens worden gebruikt om de modellen te maken en klanten score is afkomstig van een verticale CRM-oplossing met de gegevens afgeschermd ter bescherming van de privacy van klanten. Deze gegevens bevatten informatie over 8.000 abonnementen in de Verenigde Staten en het combineert drie bronnen: gegevens (metadata abonnement), activiteitsgegevens (gebruik van het systeem) en gegevens van de klant ondersteuning wordt ingericht. De gegevens niet elk bedrijf bevat relevante informatie over de klanten; bijvoorbeeld, bevat het geen metagegevens of credit scores van loyaliteit.  

Voor eenvoud, bent ETL en processen voor opschonen van gegevens buiten het bereik omdat we ervan uitgaan dat het voorbereiden van gegevens heeft al zijn klaar ergens anders.   

Functies selecteren voor het model is gebaseerd op voorlopige significantie van de set van variabelen, opgenomen in het proces dat gebruikmaakt van het forest willekeurige module scoren. We berekend voor de uitvoering in de Studio van Machine Learning, het gemiddelde, de mediaan en de bereiken voor representatieve functies. We hebben toegevoegd bijvoorbeeld aggregaten voor de kwalitatieve gegevens, zoals de minimale en maximale waarden voor de gebruikersactiviteit.    

Wij ook vastgelegd tijdelijke gegevens voor de laatste zes maanden. We gegevens geanalyseerd voor één jaar en we zelfs als er statistisch significante trends, wordt het effect op de lospeuteren aanzienlijk na zes maanden afgenomen vastgesteld.  

Het belangrijkste punt is dat het hele proces, inclusief ETL, functieselectie en modelleren van Machine Learning Studio met behulp van gegevensbronnen in Microsoft Azure is geïmplementeerd.   

De volgende diagrammen illustreren de gegevens die is gebruikt.  

![][4]

*Figuur 6: Fragment van een gegevensbron (afgeschermd)*  

![][5]


*Figuur 7: Functies die zijn opgehaald uit een gegevensbron*
 
> Houd er rekening mee dat deze gegevens persoonlijk zijn en daarom het model en de gegevens kunnen niet worden gedeeld.
> Voor een vergelijkbaar model met behulp van algemeen beschikbare gegevens, ziet in dit voorbeeld in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com/)experimenteren: [Telco klant te produceren](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Meer informatie over hoe u een lospeuteren analyse model met Cortana Intelligence Suite kunt implementeren, wordt ook aangeraden [deze video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) door Senior Program Manager Westerse Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algoritmen die worden gebruikt in het prototype

We de volgende vier machine learning algoritmen gebruikt om te bouwen van het prototype (geen aanpassing):  

1.  Logistische regressie (LR)
2.  Gestimuleerd beslissingsstructuur (BT)
3.  Gemiddelde perceptron (AP)
4.  Ondersteuning voor vector-machine (SVM)  


In het volgende diagram ziet u een gedeelte van het ontwerpvenster experiment geeft de volgorde waarin de modellen zijn gemaakt:  

![][6]  


*Figuur 8: Modellen maken in de Machine Learning Studio*  

###<a name="scoring-methods"></a>Score van methoden
We de vier modellen met behulp van een dataset gelabelde training gescoord.  

We ook de score dataset met een vergelijkbaar model gebouwd met behulp van de desktop edition van SAS Enterprise mijnwerkers 12 ingediend. Wij de juistheid van het model SAS en alle vier modellen van Machine Learning Studio gemeten.  

##<a name="results"></a>Resultaten
In deze sectie presenteren wij onze bevindingen over de nauwkeurigheid van de modellen, op basis van de score dataset.  

###<a name="accuracy-and-precision-of-scoring"></a>Nauwkeurigheid en precisie van de score
De implementatie in Azure Machine Learning is over het algemeen achter SAS nauwkeurigheid met ongeveer 10-15% (gebied onder de Curve of AUC).  

De belangrijkste metric in lospeuteren is echter de snelheid van de misclassification: dat wil zeggen, van de top N churners als voorspeld door de classificatie, die ze daadwerkelijk heeft **niet** lospeuteren en speciale behandeling nog ontvangen? In het volgende diagram worden deze misclassification koers voor alle modellen vergeleken:  

![][7]


*Figuur 9: Passau prototype gebied onder de curve*

###<a name="using-auc-to-compare-results"></a>AUC gebruiken om resultaten te vergelijken
Gebied onder Curve (AUC) is een metric die een algemene aanduiding van de *Afscheidbaarheid* tussen de verdeling van de scores voor positieve en negatieve populaties vertegenwoordigt. Is vergelijkbaar met de traditionele ontvanger exploitant kenmerk (ROC) grafiek maar één belangrijk verschil is dat de metric AUC hoeft u niet een drempelwaarde te kiezen. In plaats daarvan worden de resultaten samengevat via **alle** mogelijke keuzes. Daarentegen de traditionele ROC grafiek geeft het tarief weer positief op de verticale as en de waarde false positieve koers op de horizontale as en de drempel voor classificatie varieert.   

AUC wordt meestal gebruikt als maat voor de moeite waard om voor verschillende algoritmen (of andere systemen) omdat deze modellen worden vergeleken met behulp van de AUC-waarden toestaat. Dit is een populaire aanpak in bijvoorbeeld de meteorologische omstandigheden en de biosciences. AUC geeft dus een populaire tool voor de beoordeling van de prestaties van de classificatie.  

###<a name="comparing-misclassification-rates"></a>Misclassification tarieven vergelijken
We vergeleken de misclassification rente op de betrokken dataset met behulp van de CRM-gegevens van ongeveer 8000 abonnementen.  

-   SAS bedroeg misclassification 10 à 15%.
-   De snelheid van de misclassification Machine Learning Studio is 15-20% voor de top 200-300 churners.  

In de telecommunicatie-industrie is het belangrijk om klanten die het hoogste risico te produceren door aan te bieden ze een concierge service of andere speciale behandeling hebben. In dat verband realiseert de uitvoering van de Machine Learning Studio resultaten op het SAS-model.  

Evenzo, is nauwkeurigheid belangrijker dan de precisie omdat we vooral geïnteresseerd bent in het correct classificeren van mogelijke churners.  

De relatie in een levendige, makkelijk te begrijpen grafische afbeelding ziet u het volgende diagram uit Wikipedia:  

![][8]

*Figuur 10: Verhouding tussen de nauwkeurigheid en precisie*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Nauwkeurigheid en precisie resultaten voor gestimuleerd besluit boomstructuur  

De volgende grafiek geeft de ruwe resultaten van scoren met behulp van het prototype Machine Learning voor het model gestimuleerd besluit structuur, die de meest accurate tussen de vier modellen:  

![][9]

*Figuur 11: Gestimuleerd besluit boom model kenmerken*

##<a name="performance-comparison"></a>Vergelijking van prestaties
We vergeleken de snelheid waarmee gegevens werd gescoord aan de hand van de Machine Learning Studio modellen en een vergelijkbaar model gemaakt met behulp van de desktop-editie van SAS Enterprise mijnwerkers 12,1.  

De volgende tabel bevat een overzicht van de prestaties van de algoritmen:  

*Tabel 1. Algemene prestaties (nauwkeurigheid) van de algoritmen*

| LR|BT|AP|SVM|
|---|---|---|---|
|Gemiddelde Model|Het beste Model|Attenderen|Gemiddelde Model|

De modellen die zijn ondergebracht in Machine Learning Studio SAS ver-loop van 15-25% voor de snelheid van uitvoering, maar de nauwkeurigheid grotendeels op par is.  

##<a name="discussion-and-recommendations"></a>Discussie en aanbevelingen
In de sector telecommunicatie voortkwamen verschillende procedures voor het analyseren van lospeuteren, met inbegrip van:  

-   Maatstaven voor vier fundamentele categorieën zijn afgeleid:
    -   **Entiteit (bijvoorbeeld een abonnement)**. Algemene informatie over het abonnement en/of de klant die het voorwerp uitmaakt van lospeuteren inrichten.
    -   **Activiteit**. Alle informatie over het mogelijke gebruik die is gerelateerd aan de entiteit, bijvoorbeeld het aantal aanmeldingen krijgen.
    -   **Customer support**. Verzamelen van gegevens van de klant om aan te geven of het abonnement problemen of interacties met customer support van hadden Logboeken.
    -   **Concurrerende en zakelijke gegevens**. Alle mogelijke gegevens over de klant verkrijgen (bijvoorbeeld kunnen zijn niet beschikbaar of moeilijk te volgen).
-   Gebruik van belang voor het station functie selecteren. Dit betekent dat de boomstructuur gestimuleerd besluit is altijd een veelbelovende aanpak.  

Het gebruik van deze vier categorieën illusie de dat een eenvoudige *deterministische* benadering, op basis van de indexen gevormd op redelijke factoren per categorie, voldoende moet om te bepalen voor klanten die een risico voor lospeuteren. Helaas, hoewel dit begrip aannemelijke lijkt, is een false begrip. De reden is dat lospeuteren een tijdelijk effect is en de factoren die bijdragen te produceren meestal in tijdelijke Staten zijn. Wat kunt u vandaag nog een klant leidt mogelijk anders morgen en zeker worden verschillende zes maanden vanaf nu. Daarom een *probabilistische* model is noodzakelijk.  

Deze belangrijke waarneming in het bedrijfsleven een business intelligence-georiënteerde aanpak in het algemeen de voorkeur om analytics te vaak wordt vergeten, vooral omdat deze een eenvoudiger verkopen en eenvoudige automatisering admits.  

De belofte van analytics met behulp van Machine Learning Studio zelf is echter dat de vier categorieën van gegevens die zijn ingedeeld in de afdeling een waardevolle bron voor het leren over lospeuteren machine geworden.  

Een andere interessante mogelijkheden die Azure Machine Learning is de mogelijkheid een aangepaste module toevoegen aan de bibliotheek van vooraf gedefinieerde modules die reeds beschikbaar zijn. Deze mogelijkheid, maakt u in feite een verkoopkans selecteren bibliotheken en sjablonen maken voor verticale markten. Het is een belangrijk kenmerk van Azure Machine Learning op de markt.  

Wij hopen dat aan dit onderwerp in de toekomst, met name verband houden met big data analytics.
  
##<a name="conclusion"></a>Conclusie
Dit witboek beschrijft een verstandige aanpak van het algemene probleem van de klant lospeuteren aanpakken met behulp van een algemeen kader. We beschouwd als een prototype voor het scoren van modellen en geïmplementeerd met behulp van Azure Machine Learning. Tot slot, wij de nauwkeurigheid en prestaties van de oplossing van het prototype met betrekking tot vergelijkbare algoritmen in SAS beoordeeld.  

**Voor meer informatie:**  

Dit artikel u helpen? Geef ons uw feedback. Laat ons op een schaal van 1 (slecht) tot 5 (uitstekend), hoe beoordeelt u dit artikel en waarom hebt u gezien deze classificatie? Bijvoorbeeld:  

-   Zijn u beoordeling het hoog vanwege de goede voorbeelden, uitstekende schermopnamen duidelijk schrijven, of een andere reden?
-   Zijn u beoordeling het lage door slechte voorbeelden, fuzzy schermopnamen of onduidelijke schrijven?  

Deze feedback helpt ons bij het verbeteren van de kwaliteit van de witboeken die we vrijgeven.   

[Feedback verzenden](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Verwijzingen
Voorspellende Analytics [1] : buiten de voorspellingen, W. McKnight, Information Management, juli/augustus 2011, p.18-20.  

Wikipedia-artikel [2] : [nauwkeurigheid en precisie](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SCHERPE-DM 1.0: stapsgewijze datamining Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [grote gegevens Marketing: uw klanten effectiever te verrichten en de waarde station](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco produceren model sjabloon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Aanhangsel

![][10]

*Figuur 12: Momentopname van een presentatie op lospeuteren prototype*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
