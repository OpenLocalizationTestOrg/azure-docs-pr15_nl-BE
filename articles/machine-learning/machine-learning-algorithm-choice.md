<properties
    pageTitle="Het kiezen van machine learning algoritmen | Microsoft Azure"
    description="Het kiezen van algoritmen voor gecontroleerde en werkende learning Azure Machine Learning in clusters, classificatie of regressie proeven."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Het kiezen van algoritmen voor Microsoft Azure Machine Learning

Het antwoord op de vraag "Welke computer leren algoritme moet ik gebruiken?" is altijd "Afhankelijk is." Dit is afhankelijk van de grootte, de kwaliteit en de aard van de gegevens. Hangt af van wat u wilt doen met het antwoord. Dit is afhankelijk van hoe de berekening van het algoritme is vertaald in de instructies voor de computer die u gebruikt. Maar dit hangt af van hoeveel tijd u hebt. Zelfs de meest ervaren wetenschappers gegevens kunnen niet zien welk algoritme beste voordat u ze uitvoert.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Het algoritme van de Machine Learning cheats blad

Het **Microsoft Azure Machine leren algoritme cheats blad** helpt u bij het kiezen van de juiste machine leren algoritme voor anticiperende analytische oplossingen uit de bibliotheek met Microsoft Azure Machine Learning algoritmen.
Dit artikel helpt u bij het gebruik ervan.

> [AZURE.NOTE] Te downloaden van het blad bedriegen en overnemen van dit artikel, gaat u naar de [computer leren algoritme bedriegen blad voor Azure Machine Learning Studio](machine-learning-algorithm-cheat-sheet.md).

Dit blad bedriegen is een zeer specifieke doelgroep in gedachten: een begin gegevens wetenschappelijk met undergraduate niveau machine learning, probeert te beginnen met een algoritme kiezen in Azure Machine Learning Studio. Dit betekent dat het aantal generalisaties en oversimplifications maakt, maar het wijst u in de richting van een veilige. Het betekent ook dat er veel algoritmen die hier niet vermeld zijn. Azure Machine Learning groeien tot een volledige set beschikbare methoden omvatten, gaan we deze toevoegen.

Deze aanbevelingen zijn gecompileerde feedback en tips van een groot aantal gegevens wetenschappers en deskundigen van machine learning. We niet eens over alles, maar ik heb geprobeerd te harmoniseren onze mening in een ruwe consensus. De meeste van de overzichten van onenigheid beginnen met "Afhankelijk is..."

### <a name="how-to-use-the-cheat-sheet"></a>Het gebruik van het blad bedriegen

Het pad en de algoritme voor de labels in de grafiek als lezen ' voor * &lt;pad label&gt; * gebruik * &lt;algoritme&gt;*. " Bijvoorbeeld, *"snelheid* gebruik *twee klasse logistische regressie*." Soms meer dan één tak van toepassing.
Soms zijn ze niet past perfect. Ze zijn bedoeld als aanbevelingen van de regel van de duim, zodat u niet bang te zijn dat het exacte.
Verschillende gegevens wetenschappers die ik met bovengenoemde die gesproken de enige manier om te zoeken naar de allerbeste algoritme is om te proberen ze allemaal.

Hier volgt een voorbeeld uit de [Galerie met Cortana intelligentie](http://gallery.cortanaintelligence.com/) van een experiment dat probeert verschillende algoritmen tegen dezelfde gegevens en de resultaten vergelijkt: [vergelijken met meerdere klasse classificaties: Letter erkenning](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Als u wilt downloaden en afdrukken van een diagram een overzicht van de mogelijkheden van Machine Learning Studio geeft, Zie [diagram overzicht van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Flavors van machine learning

### <a name="supervised"></a>Onder toezicht

Leren onder toezicht algoritmen voorspellingen op basis van een aantal voorbeelden. Historische aandelenkoersen kunnen bijvoorbeeld worden gebruikt voor wat op de prijzen in de toekomst gevaar. Elk voorbeeld gebruikt voor de opleiding wordt aangeduid met de waarde van de rente, in dit geval de aandelenkoers. Een gecontroleerde leren algoritme zoekt naar patronen in de waardelabels. Gebruikmaken van alle informatie die van belang kan zijn, de dag van de week, het seizoen, de financiële gegevens van het bedrijf, het type van de industrie, de aanwezigheid van geopolicitical ontwrichtende gebeurtenissen, en elk algoritme zoekt naar verschillende soorten patronen. Nadat het algoritme is het beste patroon gevonden kan het, dat patroon wordt gebruikt om voorspellingen voor zonder label beproevings gegevens, de prijzen van morgen.

Dit is een veelgebruikt en handig type machine learning. Met één uitzondering, zijn alle modules in Azure Machine leren onder toezicht algoritmen leren. Er zijn verschillende specifieke soorten leren onder toezicht die worden vertegenwoordigd in Azure Machine leren: classificatie, regressie en afwijking detecteren.

* **Classificatie**. Wanneer de gegevens worden gebruikt voor het voorspellen van een categorie, wordt onder toezicht leren classificatie ook genoemd. Dit is het geval bij het toewijzen van een afbeelding als een afbeelding van een 'kat' of een 'hond'. Dit wordt als er slechts twee mogelijkheden, **twee klasse** of **binomiale nomenclatuur**genoemd. Als er meer categorieën als bij het voorspellen van de winnaar van het toernooi NCAA maart Madness, kan dit probleem **met meerdere klasse-indeling**wordt genoemd.

* **Regressie**. Wanneer wordt een waarde wordt voorspeld, net als bij de aandelenkoersen, gecontroleerde learning regressie genoemd.

* **Afwijking detecteren**. Soms is het doel identificeren van gegevenspunten die gewoon ongebruikelijk zijn. In de opsporing van fraude, bijvoorbeeld een zeer ongebruikelijke creditcard uitgaven patronen zijn verdacht. De mogelijke variaties zijn zo groot en ziet er zo weinig dat het niet haalbaar is om te leren welke frauduleuze activiteiten is de oefenvoorbeelden. De benadering die afwijking wordt gedetecteerd is gewoon wat normale activiteit (met behulp van een niet-frauduleuze transacties geschiedenis) ziet er en alles wat is aanzienlijk anders bepalen.

### <a name="unsupervised"></a>Werkende

Gegevenspunten hebben werkende leren, geen labels gekoppeld. In plaats daarvan is het doel van een zonder toezicht werkende leren algoritme om de gegevens op een bepaalde manier of om de structuur te beschrijven. Dit kan betekenen in clusters groeperen of het vinden van de verschillende manieren van kijken naar complexe gegevens zodat het eenvoudiger of meer geordend wordt weergegeven.

### <a name="reinforcement-learning"></a>Versterking leren

Het algoritme wordt in versterking leren, een actie te kiezen in reactie op elk gegevenspunt. Het algoritme leren ontvangt ook een beloning signaal korte tijd later, die aangeeft hoe goed de beslissing is.
Het algoritme wijzigt op basis hiervan de strategie om te komen tot de hoogste prijs. Er zijn momenteel geen versterking algoritme modules in Azure Machine leren leren. Versterking leerproces is gebruikelijk in robotica, waarbij de set van sensor gemeten op een bepaald tijdstip een gegevenspunt en het algoritme moet kiezen de volgende actie van de robot. Het is ook dat een natuurlijke geschikt voor Internet dingen toepassingen.

## <a name="considerations-when-choosing-an-algorithm"></a>Overwegingen bij het kiezen van een algoritme

### <a name="accuracy"></a>Nauwkeurigheid

Ophalen van de meest nauwkeurige antwoord mogelijk is niet altijd nodig.
Een benadering is soms voldoende zijn, afhankelijk van wat u wilt gebruiken. Als dit het geval is, kunt u mogelijk de verwerkingstijd aanzienlijk knippen door meer bij benadering methoden neer. Een ander voordeel van meer bij benadering methoden is dat ze natuurlijk vaak [overfitting](https://youtu.be/DQWI1kvmwRg)voorkomen.

### <a name="training-time"></a>Trainingstijd

Het aantal minuten of uren nodig om een model van de trein verschillen veel algoritmen. Opleiding tijd is vaak nauw verbonden met nauwkeurigheid: een meestal wordt geleverd bij de andere. Bovendien zijn sommige algoritmen gevoeliger zijn voor het aantal gegevenspunten dan andere.
Als de tijd beperkt is kan deze de keuze van het algoritme, station vooral wanneer de gegevensset groot is.

### <a name="linearity"></a>Lineariteit

Partijen van machine learning algoritmen maken gebruik van de lineariteit. Lineaire indeling algoritmen wordt ervan uitgegaan dat de klassen kunnen worden gescheiden door een rechte lijn (of de hogere dimensionale analoge). Dit zijn logistische regressie en support vector machines (zoals geïmplementeerd in Azure Machine Learning).
Algoritmen voor lineaire regressie wordt ervan uitgegaan dat de trends van een rechte lijn volgen. Deze hypothesen niet slecht voor enkele problemen, maar op andere ze nauwkeurigheid omlaag te brengen.

![Niet-lineaire klasse bounday][1]

***Niet-lineaire klasse grens*** *-vertrouwen op een lineaire indeling algoritme zou resulteren in een lage nauwkeurigheid*

![Gegevens met een niet-lineaire trend][2]

***Gegevens met een niet-lineaire trend*** *-met behulp van een lineaire regressiemethode zou genereren veel fouten groter dan nodig is*

Ondanks de gevaren zijn lineaire algoritmen erg populair als eerste regel van een aanval. Ze zijn meestal algoritmisch eenvoudig en snel te trainen.

### <a name="number-of-parameters"></a>Aantal parameters

Parameters zijn de knoppen die een wetenschappelijk gegevens krijgt u bij het instellen van een algoritme. Dit zijn getallen die van invloed zijn op de werking van het algoritme, zoals fouttolerantie of het aantal iteraties, opties of tussen de varianten van de werking van het algoritme. De trainingstijd en de nauwkeurigheid van het algoritme kunnen soms zijn zeer gevoelig aan om alleen de juiste instellingen. Algoritmen met grote aantallen parameters moeten normaal gesproken de meeste proefondervindelijk te vinden van een goede combinatie.

Er is ook een module [parameter verstrekkende](machine-learning-algorithm-parameters-optimize.md) blok in Azure Machine Learning waarmee wordt geprobeerd automatisch alle combinaties van parameters op de granulatie dat u kiest. Dit is een uitstekende manier om te controleren of dat u hebt de parameter ruimte omspannen, de benodigde tijd voor het trainen van een model wordt verhoogd exponentieel met het aantal parameters.

De opwaartse aard is dat meestal veel parameters die geeft aan dat een algoritme meer flexibiliteit heeft. Vaak kunt zeer goede nauwkeurigheid bereiken. Die vindt u de juiste combinatie van de instellingen.

### <a name="number-of-features"></a>Aantal functies

Voor bepaalde typen gegevens, het aantal functies kan erg groot worden in vergelijking met het aantal gegevenspunten. Dit is vaak het geval met genetica of tekstgegevens. Het grote aantal functies kunt u sommige algoritmen leren, waardoor de training unfeasibly lange tijd bog. Support Vector Machines zijn bijzonder goed geschikt voor dit geval (Zie hieronder).

### <a name="special-cases"></a>Speciale gevallen

Sommige algoritmen leren maken bepaalde veronderstellingen over de structuur van de gegevens of de gewenste resultaten. Als u die aansluit bij uw behoeften vinden kunt, krijgt deze u meer resultaat, meer nauwkeurige voorspellingen of sneller opleiding tijden.

|**Algoritme**|**Nauwkeurigheid**|**Trainingstijd**|**Lineariteit**|**Parameters**|**Notities**|
|---|:---:|:---:|:---:|:---:|---|
|**Twee klasse-indeling**| | | | | |
|[logistische regressie](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[Beschikking van de forest](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[Beschikking van de jungle](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Weinig geheugen footprint|
|[beslissingsstructuur gestimuleerd](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Groot geheugen footprint|
|[neural network](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Verdere aanpassing is mogelijk.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[gemiddelde perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[ondersteuning voor vector-machine](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Geschikt voor grote functiesets|
|[lokaal diep support vector machine](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Geschikt voor grote functiesets|
|[Bayes punt machine](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Meerdere klasse indeling**| | | | | |
|[logistische regressie](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[Beschikking van de forest](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[Beschikking van de jungle](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Weinig geheugen footprint|
|[neural network](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Verdere aanpassing is mogelijk.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[een-v-alles](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Zie eigenschappen van de geselecteerde twee klasse-methode|
|**Regressie**| | | | | |
|[lineaire](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Lineaire Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[Beschikking van de forest](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[beslissingsstructuur gestimuleerd](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Groot geheugen footprint|
|[snelle forest kwantiel](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Verdelingen in plaats van voorspellingen punt|
|[neural network](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Verdere aanpassing is mogelijk.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Technisch log-lineaire. Voor het voorspellen van tellingen|
|[rangtelwoord](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Voor het voorspellen van rang te bestellen|
|**Afwijking detectie**| | | | | |
|[ondersteuning voor vector-machine](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Vooral geschikt voor grote functiesets|
|[Afwijking op basis van Pso detectie](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-middelen](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Een algoritme voor clustering|


**Algoritme-eigenschappen:**

**●** - geeft uitstekende nauwkeurigheid, snelle opleiding tijden en het gebruik van de lineariteit

**○** - geeft goede nauwkeurigheid en matige training tijden

## <a name="algorithm-notes"></a>Algoritme voor notities

### <a name="linear-regression"></a>Lineaire regressie

Zoals eerder vermeld, past [lineaire regressie](https://msdn.microsoft.com/library/azure/dn905978.aspx) in een lijn (of vlak, of hyperplane) voor de gegevensset. Het is een werkpaard, eenvoudig en snel, maar het kan veel te eenvoudig voor sommige problemen.
Schakel dit selectievakje in voor een [zelfstudie met lineaire regressie](machine-learning-linear-regression-in-azure.md).

![Gegevens met een lineaire trend][3]

***Gegevens met een lineaire trend***

### <a name="logistic-regression"></a>Logistische regressie

Hoewel deze verwarrende 'regressie' in de naam bevat, is logistische regressie daadwerkelijk een krachtig instrument voor de classificatie van [twee klasse](https://msdn.microsoft.com/library/azure/dn905994.aspx) - en [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) . Het is snel en eenvoudig. Het feit dat deze gebruikmaakt van een van '-shaped curve in plaats van een rechte lijn kunt u naadloos voor het verdelen van gegevens in groepen. Logistische regressie geeft lineaire klassengrenzen, dus moeten wanneer u gebruikt, dat een lineaire benadering is iets dat die u kunt leven.

![Logistische regressie met twee klasse gegevens met slechts één functie][4]

***Een logistische regressie met twee klasse gegevens met slechts één functie*** *-de rand van de klasse is het punt waarop een logistieke kromme is net zo dicht bij beide klassen*

### <a name="trees-forests-and-jungles"></a>Bomen, bossen en jungles

Beschikking van de bossen ([regressie](https://msdn.microsoft.com/library/azure/dn905862.aspx), [twee klasse](https://msdn.microsoft.com/library/azure/dn906008.aspx)en [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), besluit jungles ([twee klasse](https://msdn.microsoft.com/library/azure/dn905976.aspx) en [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) en gestimuleerd beslissingsstructuren ([regressie](https://msdn.microsoft.com/library/azure/dn905801.aspx) en [twee klasse](https://msdn.microsoft.com/library/azure/dn906025.aspx)) zijn alle gebaseerd op beslissingsstructuren, een basissysteem machine learning concept. Er zijn veel varianten van beslissingsstructuren, maar ze voeren allemaal hetzelfde, de functie space onderverdelen in regio's met grotendeels dezelfde label. Deze kunnen regio's van dezelfde categorie of constante waarde, afhankelijk van of u classificatie of regressie doet zijn.

![Beslissingsstructuur verdeelt de ruimte van een functie][5]

***Een beslissingsstructuur verdeelt de ruimte voor een functie in regio's van de min of meer uniform***

Omdat een functie ruimte kan worden onderverdeeld in willekeurig kleine regio's, het is gemakkelijk te stel fijn genoeg delen één gegevenspunt hebt per regio, een extreem voorbeeld van te. Om dit te vermijden, zijn een groot aantal bomen met speciale wiskundige zorg genomen samengesteld dat de bomen niet aan elkaar zijn gekoppeld. Het gemiddelde van dit forest"beslissing" is een structuur die wordt te voorkomen. Beschikking van de bossen kunnen veel geheugen gebruiken. Beschikking van de jungles zijn een variant die minder geheugen ten koste van de iets meer trainingstijd in beslag.

Beslissingsstructuren gestimuleerd voorkomen te door te beperken hoe vaak ze kunnen onderverdelen en hoe weinig gegevenspunten zijn toegestaan in elke regio. Het algoritme wordt een reeks van bomen, die allemaal aanleren ter compensatie van de fout gegeven door de structuur voor. Het resultaat is een zeer nauwkeurige cursist die vaak veel geheugen gebruikt. Uitchecken voor de volledige technische beschrijving [van Friedman oorspronkelijke papier](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Snelle forest kwantiel regressie](https://msdn.microsoft.com/library/azure/dn913093.aspx) is een variatie van beslissingsstructuren voor het speciale geval waar u niet alleen de typische (mediaan) waarde van de gegevens in een regio, maar ook de distributie in de vorm van quantiles kent.

### <a name="neural-networks-and-perceptrons"></a>Neural networks en perceptrons

Neural networks zijn hersenen geïnspireerd algoritmen die betrekking hebben op [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx)en [twee klasse](https://msdn.microsoft.com/library/azure/dn905947.aspx) [regressie](https://msdn.microsoft.com/library/azure/dn905924.aspx) problemen leren. Komen in een oneindige verscheidenheid, maar de neural networks in Azure Machine Learning zijn alle van de vorm van gestuurde acyclische grafieken. Dat houdt in dat invoer worden doorgegeven naar voren (nooit terug) via een reeks van lagen voordat het wordt omgezet in outputs. In elke laag, zijn "inputs" in verschillende combinaties worden gewogen, opgeteld en doorgegeven aan de volgende laag. Deze combinatie van eenvoudige berekeningen resulteert in de mogelijkheid om voor meer geavanceerde klasse begrenzingen en gegevens trends schijnbaar magic. Veel lagen netwerken van dergelijke voeren de "grondige kennis" die helpt u zo veel technische rapportage en science fiction.

Deze krachtige niet afkomstig zijn gratis, maar. Neural networks kunnen lang duren om te trainen, met name voor grote gegevenssets met een groot aantal functies. Zij hebben ook meer parameters dan de meeste algoritmen, wat betekent dat de Veegpoeder parameter de trainingstijd veel groter.
En zijn de mogelijkheden voor deze overachievers die [hun eigen netwerkstructuur opgeven willen](http://go.microsoft.com/fwlink/?LinkId=402867), inexhaustible.

<a name="boundaries-learned-by-neural-networks6"></a>![Grenzen geleerd van neural networks][6]
---------------------------

***De grenzen van neural networks geleerd zijn complex en onregelmatige***

De [twee klasse gemiddeld perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) is neural networks antwoord op de training vaak enorm. Een netwerkstructuur waarmee lineaire klassengrenzen worden gebruikt. Het is bijna primitieve volgens de normen van vandaag, maar heeft een lange geschiedenis van krachtig werken en klein genoeg is om te leren snel.

### <a name="svms"></a>SVMs

Support vector machines (SVMs) vinden de grens tussen klassen door als brede marge mogelijk. Wanneer de twee klassen duidelijk kunnen worden gescheiden, vinden de algoritmen het beste grenzen ze kunnen. Zoals geschreven in Azure Machine Learning, de [twee klasse SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) gebeurt met rechte lijn. (In SVM spreken wordt gebruikt een lineaire kernel.) Omdat het deze lineaire aanpassing maakt, is het vrij snel uitvoeren. Met de functie intense gegevens, zoals tekst of genomica is waar het een uitkomst. In dat geval zijn de SVMs klassen kunnen sneller en met minder te dan de meeste andere algoritmen, naast een bescheiden hoeveelheid geheugen vereist.

![Support vector machine klasse grens][7]

***Een typische support vector machine klasse grens maximaliseert de marge tussen twee klassen***

Een ander product van Microsoft Research, de [twee klasse lokaal diep SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) is een niet-lineaire variant van SVM dat de meeste van de snelheid en efficiëntie van de lineaire versie behoudt. Het is ideaal voor gevallen waarin de lineaire benadering een voldoende nauwkeurige antwoorden krijgt. De ontwikkelaars bewaard het snel door het probleem splitsen in een aantal kleine problemen met lineaire SVM. Lees de [volledige beschrijving](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) voor de details over hoe ze uit deze truc getrokken.

Met behulp van een slimme uitbreiding van niet-lineaire SVMs tekent het [één klasse SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) een grens die strak geeft een overzicht van de hele gegevensset. Het is nuttig voor de detectie van de afwijking. Een nieuwe gegevenspunten die ver buiten deze begrenzing vallen zijn ongebruikelijk zijn opmerkelijk.

### <a name="bayesian-methods"></a>Bayesian methoden

Bayesian methoden beschikt over een zeer wenselijk: deze te voorkomen. Zij doet dit door bepaalde veronderstellingen vooraf over de waarschijnlijke verspreiding van het antwoord. Een ander bijkomend gevolg van deze aanpak is dat ze zeer weinig parameters. Azure Machine Learning biedt zowel Bayesian algoritmen voor classificatie ([twee klasse Bayes punt machine](https://msdn.microsoft.com/library/azure/dn905930.aspx)) en regressie ([lineaire regressie van Bayesiaanse](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Houd er rekening mee dat deze wordt ervan uitgegaan dat de gegevens kunnen worden gesplitst of met een rechte lijn aanpassen.

Bayes punt machines zijn op een historische notitie bij Microsoft Research ontwikkeld. Deze hebben uitzonderlijk mooie theoretische werk achter hen. De betrokken student is gericht op het [oorspronkelijke artikel in JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) en op een [begrijpelijke manier mee blog door Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Speciale algoritmen

Als u een zeer specifiek doel hebt u mogelijk mee. Binnen de collectie Azure Machine Learning zijn algoritmen die zijn gespecialiseerd in rang voorspelling ([ordinale regressie](https://msdn.microsoft.com/library/azure/dn906029.aspx)) en aantal voorspelling ([Poisson regressie](https://msdn.microsoft.com/library/azure/dn905988.aspx)) afwijking detectie (een op basis van de [principal components analysis](https://msdn.microsoft.com/library/azure/dn913102.aspx) en gebaseerd op een [machine vector ondersteuning](https://msdn.microsoft.com/library/azure/dn913103.aspx)s).
En er is een alleenstaande clustering algoritme ook ([K-middelen](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![Afwijking op basis van Pso detectie][8]

***Afwijking op basis van Pso detectie*** *-de overgrote meerderheid van de gegevens worden onderverdeeld in een stereotypical verdeling; punten die afwijken aanzienlijk van dat distributie verdacht zijn*

![Data set K wijze gegroepeerd][9]

***Een gegevensset is ingedeeld in 5 clusters met gebruikmaking van K***

Er is ook een ensemble [een-v-alles multiclass-classificatie](https://msdn.microsoft.com/library/azure/dn905887.aspx), die het probleem van de indeling N-klasse in twee klasse-indeling problemen met N-1 opgesplitst. De nauwkeurigheid, de trainingstijd en de lineariteit eigenschappen worden bepaald door de twee klasse classificaties gebruikt.

![Twee klasse classificaties worden gecombineerd om een classificatie van drie-klasse][10]

***Een paar van twee klasse classificaties combineren om een classificatie van drie-klasse***

Azure Machine Learning tevens toegang tot een krachtige machine learning kader onder de titel van [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW defies categorisatie hier, omdat het meer indelings- en regressie problemen en zelfs van gegevens gedeeltelijk zonder label leren kunt. U kunt configureren voor het gebruik van een van een aantal algoritmen, verlies functies en optimalisatie algoritmen te leren. Is ontworpen vanaf de grond omhoog om efficiënt, parallelle en zeer snel. Komisch grote functiesets moeiteloos zichtbaar worden verwerkt.
Gestart en geleid door Microsoft Research van eigen John Langford, is de VW een formule een post in een veld van het voorraad auto-algoritmen. Niet elk probleem past VW, maar als uw bedrijf is, kan het zijn waard uw tijd om te leren klimt op de interface. Het is ook beschikbaar als [zelfstandige open broncode](https://github.com/JohnLangford/vowpal_wabbit) in verschillende talen.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
