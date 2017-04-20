<properties 
    pageTitle="Handleiding voor de Net # Neural Networks specificatietaal | Microsoft Azure" 
    description="Syntaxis voor de Net # neural networks specificatietaal met voorbeelden van hoe u een aangepaste neural network om model te maken in Microsoft Azure ML met behulp van Net#" 
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
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Gids voor Net # neural network specificatietaal voor Azure Machine Learning

## <a name="overview"></a>Overzicht
NET # is een taal die is ontwikkeld door Microsoft, dat is gebruikt voor het definiëren van neural netwerkarchitecturen voor neural network modules in Microsoft Azure Machine Learning. In dit artikel wordt beschreven:  

-   Basisbegrippen voor neural networks
-   Neural network-vereisten en het definiëren van de primaire onderdelen
-   De syntaxis en de trefwoorden van de taal Net #-specificatie
-   Voorbeelden van aangepaste neural networks gemaakt met Net# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Neural network grondbeginselen
Een neural network structuur bestaat uit de ***knooppunten*** die zijn geordend in ***lagen***, en gewogen ***verbindingen*** (of ***randen***) tussen de knooppunten. De verbindingen zijn gericht en elke verbinding heeft een ***bron*** en een ***bestemming*** knooppunten.  

Elke ***laag trainable*** (een verborgen of een laag uitvoer) heeft een of meer ***verbindingen bundels***. Een bundel verbinding bestaat uit een bronlaag en een specificatie van de verbindingen van die bronlaag. Alle verbindingen in een bundel van bepaalde delen dezelfde ***bronlaag*** en dezelfde ***doellaag***. In Net # een bundel verbinding beschouwd als behorend tot de bundel van de doellaag.  
 
NET # ondersteunt diverse soorten verbinding bundels waarmee u het aanpassen van de manier waarop invoer zijn toegewezen aan verborgen lagen en toegewezen op de uitgangen.   

De standaard of standaard bundel is een **volledige bundel**, waarin elk knooppunt in de bronlaag is aangesloten op elk knooppunt in de doellaag.  

Bovendien ondersteunt Net # vier soorten geavanceerde verbinding bundels:  

-   **Gefilterde bundels**. De gebruiker kan een predicaat definiëren met behulp van de locaties van het bronknooppunt laag en het doelknooppunt laag. Knooppunten zijn verbonden wanneer het predicaat waar is.
-   **Convolutional bundels**. De gebruiker kan kleine groepen van knooppunten in de bronlaag definiëren. Elk knooppunt in de doellaag is verbonden met een groep van knooppunten in de bronlaag.
-   **Pooling bundels** en **antwoord normalisatie bundels**. Deze zijn vergelijkbaar met convolutional bundels in kleine groepen van knooppunten in de bronlaag definieert. Het verschil is het gewicht van de randen in deze pakketten zijn niet trainable. In plaats daarvan wordt een vooraf gedefinieerde functie op in de waarden van de bron om te bepalen van de waarde van het knooppunt bestemming toegepast.  

Net # definiëren met de structuur van een neural network kunt u complexe structuren zoals diep neural networks of convoluties van willekeurige dimensies, waarvan bekend is dat het verbeteren van de kennis op gegevens, zoals een afbeelding, audio of video definiëren.  

## <a name="supported-customizations"></a>Ondersteunde aanpassingen
De architectuur van neural network modellen die u in Azure Machine Learning maakt kan uitgebreid worden aangepast met behulp van Net #. U kunt:  

-   Verborgen lagen maken en het aantal knooppunten in elke laag.
-   Opgeven hoe lagen met elkaar worden verbonden.
-   Connectiviteit met speciale structuren, zoals convoluties en gewicht delen bundels definiëren.
-   Geef de activering van verschillende functies.  

Zie voor meer informatie over de syntaxis van de specificatie, [Structuur-specificatie](#Structure-specifications).  
 
Zie de [voorbeelden](#Examples-of-Net#-usage)voor voorbeelden van het definiëren van neural networks voor sommige gemeenschappelijke computer taken van Simplex () tot complexe, leren.  

## <a name="general-requirements"></a>Algemene voorschriften
-   Moet er precies één uitgang laag, ten minste één invoer laag en nul of meer verborgen lagen. 
-   Elke laag heeft een vast aantal knooppunten, conceptueel gerangschikt in een rechthoekige matrix met willekeurige dimensies. 
-   Invoer lagen hebben geen bijbehorende parameters opgeleide en vertegenwoordigen het punt waarbij gegevens invoert in het netwerk. 
-   Trainable lagen (de uitvoer- en verborgen lagen) gekoppeld opgeleide parameters, systematische en gewichten genoemd. 
-   De knooppunten van de bron- en doeladressen moeten in afzonderlijke lagen. 
-   Verbindingen moet acyclische; Er kan niet met andere woorden, een keten van verbindingen die leiden naar het bronknooppunt van de oorspronkelijke.
-   De uitvoer laag kan niet een bronlaag van een bundel van de verbinding.  

## <a name="structure-specifications"></a>Structuur-specificaties
Een specificatie neural network structuur bestaat uit drie gedeelten: de **declaratie van de constante**, de **laag verklaring**, de **verklaring van de verbinding**. Er is ook een sectie optionele **aangifte delen** . De secties kunnen in willekeurige volgorde worden opgegeven.  

## <a name="constant-declaration"></a>Declaratie van de constante 
Een constante declaratie is optioneel. Het biedt een manier voor het definiëren van waarden die elders worden gebruikt in de definitie van neural network. De declaratie-instructie bestaat uit een id die wordt gevolgd door een gelijkteken en een waarde-expressie.   

De volgende instructie wordt bijvoorbeeld een constante **x**gedefinieerd:  


    Const X = 28;  

Als u twee of meer constanten tegelijkertijd definieert, plaatst u de id-namen en waarden tussen accolades en deze scheiden met puntkomma's. Bijvoorbeeld:  

    Const { X = 28; Y = 4; }  

De rechterkant van de toewijzingsexpressie van elke kan een geheel getal, een reëel getal, een Boole-waarde (True of False) of een wiskundige expressie zijn. Bijvoorbeeld:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Verklaring van laag
De verklaring van de laag is vereist. De grootte en de bron van de laag, met inbegrip van de verbinding bundels en kenmerken worden gedefinieerd. De declaratie-instructie begint met de naam van de laag (invoer, verborgen of uitvoer), gevolgd door de afmetingen van de laag (een tupel van positieve gehele getallen). Bijvoorbeeld:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Het product van de dimensies is het aantal knooppunten in de laag. In dit voorbeeld zijn er twee dimensies [5,20], wat betekent dat er 100 knooppunten in de laag zijn.
-   Lagen kunnen worden gedeclareerd in willekeurige volgorde, met één uitzondering: de volgorde waarin ze zijn gedeclareerd als meer dan één invoer laag is gedefinieerd, moet overeenkomen met de volgorde van de functies in de invoergegevens.  


Als u wilt opgeven dat het aantal knooppunten in een laag automatisch worden bepaald, gebruikt u het sleutelwoord **auto** . Het sleutelwoord **auto** heeft verschillende effecten, afhankelijk van de laag:  

-   Het aantal knooppunten is in de aangifte van een laag van invoer het aantal functies in de invoergegevens.
-   Het aantal knooppunten is in een verborgen laag aangifte, het nummer dat is opgegeven door de waarde van de parameter voor **het aantal verborgen knooppunten**. 
-   Het aantal knooppunten is in een aangifte van uitvoer laag, 2 voor twee klasse-indeling, 1 voor regressie en gelijk is aan het aantal knooppunten uitvoer voor de indeling multiclass.   

De volgende definitie van netwerk kunt bijvoorbeeld het formaat van alle lagen automatisch worden bepaald:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Een verklaring van de laag voor een trainable laag (de uitvoer of verborgen lagen) kan desgewenst de output functie (ook wel een functie activeren), die standaard **sigmoid** voor classificatie modellen en **lineaire** voor regressie-modellen. (Zelfs als u de standaardwaarde wilt gebruiken, u kunt expliciet aangeven de functie activeren indien gewenst voor de duidelijkheid.)

De volgende uitvoer functies worden ondersteund:  

-   sigmoid
-   lineaire
-   softmax
-   rlinear
-   vierkant
-   WORTEL
-   srlinear
-   ABS
-   TANH 
-   brlinear  

Zo gebruikt de volgende declaratie voor de functie **softmax** :  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Verklaring van de verbinding
Onmiddellijk na de trainable laag definieert, moet u de verbindingen tussen de lagen die u hebt gedefinieerd declareren. De verklaring van de bundel verbinding begint met het sleutelwoord **uit**, gevolgd door de naam van de bundel de bronlaag en de aard van de bundel verbinding te maken.   

Op dit moment worden verbinding bundels van vijf typen ondersteund:  

-   **Volledige** bundels, aangegeven door het sleutelwoord **alle**
-   **Gefilterde** bundels, aangegeven door het trefwoord **waar**, gevolgd door een expressie voor een predikaat
-   **Convolutional** bundels, aangegeven door het sleutelwoord **convolve**, gevolgd door het convolutiefilter kenmerken
-   **Pooling** bundels, aangegeven door de trefwoorden **max van toepassingen** of **toepassingen betekenen**
-   **Antwoord normalisatie** bundels, aangegeven door het sleutelwoord **antwoord norm**      

## <a name="full-bundles"></a>Volledige bundels  

Een volledige verbinding bundel omvat een verbinding vanaf elk knooppunt in de bronlaag op elk knooppunt in de doellaag. Dit is het type netwerkverbinding standaard.  

## <a name="filtered-bundles"></a>Gefilterde bundels
Een gefilterde verbinding bundel-specificatie bevat een deel van het predikaat, syntactisch, uitgedrukt als een C# lambda-expressie. In het volgende voorbeeld definieert twee gefilterde bundels:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   **S** is een parameter die een index vertegenwoordigt de rechthoekige matrix van knooppunten van de invoer laag, de _Pixels_in het predicaat voor _ByRow_en **d** is een parameter die een index vertegenwoordigt in de matrix van knooppunten van de verborgen laag, _ByRow_. Het type van **s** en **d** is een tupel van gehele getallen van twee lengte. In dat geval **s** bereiken via alle paren van gehele getallen met _0 < = s [0] < 10_ en _0 < =[1] s < 20_, en **d** bereiken via alle paren van gehele getallen, met _0 < = d [0] < 10_ en _0 < = d[1] < 12_. 
-   Er is een voorwaarde aan de rechterkant van de expressie predikaatfunctie. In dit voorbeeld wordt voor elke waarde van **s** en **d** , dat de voorwaarde waar is, er is een rand van het bronknooppunt laag naar het doelknooppunt laag. Deze filterexpressie geeft dus aan dat de bundel omvat een verbinding van het knooppunt dat wordt gedefinieerd door **s** naar het knooppunt dat wordt gedefinieerd door **d** in alle gevallen waarbij s [0] gelijk aan d [0 is].  

Desgewenst kunt u een set van gewichten voor een gefilterde bundel. De waarde voor het kenmerk **gewichten** moet een tupel van drijvende-kommagetallen met een lengte die overeenkomt met het aantal verbindingen dat is gedefinieerd door de bundel. Het gewicht wordt standaard willekeurig gegenereerd.  

Gewichtswaarden gegroepeerd door de index van de doel-knooppunt. Dat wil zeggen, als het eerste bestemmingsknooppunt is verbonden met K bron knooppunten, zijn de eerste _K_ van de tupel **gewichten** de gewichten voor de eerste doelknooppunt in volgorde van gegevensbron index. Hetzelfde geldt voor de resterende knooppunten van de bestemming.  

Het is mogelijk het gewicht rechtstreeks als constante waarden opgeven. Bijvoorbeeld als u de gewichten eerder hebt geleerd, kunt u deze als constanten, gebruik de volgende syntaxis:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional bundels
Wanneer de gegevens van de opleiding een homogene structuur heeft, worden vaak convolutional verbindingen voor meer informatie over geavanceerde functies van de gegevens gebruikt. Gegevens bijvoorbeeld in een afbeelding, audio of video, ruimtelijke of tijdelijke dimensionaliteit kunnen zijn vrij uniform.  

Convolutional bundels gebruikmaken van rechthoekige **kernels** die via de dimensies worden geschoven. In principe definieert elke kernel een set van gewichten toegepast in lokale groepen, aangeduid als een **kernel toepassingen**. Elke toepassing kernel correspondeert met een knooppunt in de bronlaag waarnaar wordt verwezen als het **centrale knooppunt**. Het gewicht van een kernel worden gedeeld door het aantal verbindingen. Elke kernel is rechthoekig in een bundel convolutional, en alle kernel toepassingen hebben dezelfde grootte.  

Bundels convolutional ondersteunen de volgende kenmerken:

**InputShape** definieert de dimensionaliteit van de bronlaag voor de toepassing van deze convolutional bundel. De waarde moet een tupel van positieve gehele getallen. Het aantal knooppunten in de bronlaag moet gelijk zijn aan het product van de gehele getallen, maar in andere gevallen hoeft niet overeenkomt met de aangegeven voor de bronlaag dimensionaliteit. De lengte van deze tupel als **ariteit** waarde voor de convolutional-bundel. (Meestal ariteit verwijst naar het aantal argumenten of operanden die een functie kunt uitvoeren.)  

Gebruik de shape en de locaties van de kernels definiëren de kenmerken **KernelShape**, **Stapsgewijze** **Padding**, **LowerPad**en **UpperPad**:   

-   **KernelShape**: (vereist) definieert de dimensionaliteit van elke kernel voor de bundel convolutional. De waarde moet een tupel van positieve gehele getallen met een lengte die gelijk is aan de ariteit van de bundel. Elk onderdeel van deze tupel moet niet groter zijn dan het overeenkomstige onderdeel van **InputShape**. 
-   **Stapsgewijze**: (optioneel) definieert de verschuivende stap grootte van het convolutiefilter (één stapgrootte voor elke dimensie), dat is de afstand tussen de knooppunten van de centrale. De waarde moet een tupel van positieve gehele getallen met een lengte die de ariteit is van het desbetreffende pakket. Elk onderdeel van deze tupel moet niet groter zijn dan het overeenkomstige onderdeel van **KernelShape**. De standaardwaarde is een tupel met alle onderdelen die gelijk is aan een. 
-   **Delen**: (optioneel) definieert het gewicht voor elke dimensie van het convolutiefilter delen. De waarde kan een enkele Booleaanse waarde of een tupel van Boole-waarden met een lengte van de ariteit van de bundel zijn. Een enkele Booleaanse waarde is uitgebreid met een tupel van de juiste lengte waarbij alle onderdelen die gelijk is aan de opgegeven waarde. De standaardwaarde is een tupel die uit alle True-waarden bestaat. 
-   **MapCount**: (optioneel) bepaalt het aantal functie worden toegewezen voor de convolutional-bundel. De waarde kan zijn voor een enkel positief geheel getal of een tupel van positieve gehele getallen met een lengte van de ariteit van de bundel. Een enkel geheel getal is uitgebreid met een tupel van de juiste lengte onderdelen van het eerste gelijk is aan de opgegeven waarde en de resterende onderdelen gelijk is aan een. De standaardwaarde is één. Het totale aantal kaarten van de functie is het product van de onderdelen van de tupel. Factoring van dit totaal over de onderdelen wordt bepaald hoe de functie Overzicht waarden worden gegroepeerd in de knooppunten van de bestemming. 
-   **Gewicht**: (optioneel) definieert de initiële gewichten voor de bundel. De waarde moet een tupel van drijvende-kommagetallen met een lengte van het aantal keren kernels het aantal of gewicht per kernel, zoals verderop in dit artikel omschreven. De gewichten standaard willekeurig wordt gegenereerd.  

Er zijn twee sets eigenschappen die opvulling, bepalen de eigenschappen worden die elkaar wederzijds uitsluiten:

-   **Padding**: (optioneel) bepaald of de invoer moet worden aangevuld met behulp van een **standaardschema voor opvulling**. De waarde kan een enkele Booleaanse waarde zijn of kan een tupel van Boole-waarden met een lengte die de ariteit is van het desbetreffende pakket. Een enkele Booleaanse waarde is uitgebreid met een tupel van de juiste lengte waarbij alle onderdelen die gelijk is aan de opgegeven waarde. Als de waarde voor een dimensie ingesteld op True is, wordt de bron logisch in die dimensie met de waarde nul cellen ter ondersteuning van aanvullende kernel toepassingen, aangevuld, dat de centrale knooppunten van het eerste en laatste kernels in die dimensie de knooppunten van het eerste en laatste in die dimensie in de bronlaag zijn. Dus het aantal knooppunten dat 'dummy' in elke dimensie wordt automatisch bepaald, past precies _(InputShape [d] - 1) / stapsgewijze [d] + 1_ kernels in de bronlaag gevuld. Als de waarde voor een dimensie ingesteld op False is, wordt de kernels worden gedefinieerd zodat het aantal knooppunten aan weerszijden zijn weggelaten hetzelfde (tot een verschil van 1 is). De standaardwaarde van dit kenmerk is een tupel met alle onderdelen die gelijk is aan False.
-   **UpperPad** en **LowerPad**: (optioneel) bieden meer controle over de hoeveelheid opvulling te gebruiken. **Belangrijk:** Deze kenmerken kunnen worden gedefinieerd als de eigenschap **Padding** hierboven is ***niet*** gedefinieerd. De waarden moeten worden tuples met een lengte die de ariteit van de bundel zijn gehele getallen. Als deze kenmerken zijn opgegeven, worden 'dummy' knooppunten toegevoegd aan de onderste en bovenste uiteinden van elke dimensie van de input laag. Het aantal knooppunten toegevoegd aan de onderste en bovenste uiteinden in elke dimensie wordt bepaald door **LowerPad**[i] en **UpperPad**[i]. Om ervoor te zorgen dat kernels alleen knooppunten 'echte' en niet 'dummy' knooppunten overeenkomen, moeten aan de volgende voorwaarden worden voldaan:
    -   Elke component van de **LowerPad** moet strikt minder dan KernelShape [d] / 2. 
    -   Elke component van de **UpperPad** niet groter zijn dan KernelShape [d] / 2. 
    -   De standaardwaarde van deze kenmerken is een tupel met alle onderdelen die gelijk is aan 0. 

De instelling van de **Opvulling** = true kan zoveel opvulling te houden van het "center" van de kernel in de "reële" invoer nodig is. Hiermee wijzigt u de wiskunde iets voor het berekenen van de grootte. In het algemeen, de grootte _D_ wordt berekend als _D = (I - K) / S + 1_, waar _ik_ de grootte van de invoer is _K_ is de grootte van de kernel, _S_ de stride, en _/_ delen (ronde naar nul) is. Als u UpperPad = [1, 1], de invoer _ik_ is effectief 29, en dus _D = (29-5) / 2 + 1 = 13_. Echter, als **Opvulling** = true, in feite _ik_ haalt tegenaan omhoog door _K - 1_; dus _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Door het opgeven van waarden voor **UpperPad** en **LowerPad** dat u veel meer controle over de opvulling dan als u **opvulling instellen** = true.

Raadpleeg de volgende artikelen voor meer informatie over convolutional, netwerken en hun toepassingen:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.Microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.MIT.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Pooling bundels
Een **bundel pooling** van toepassing is vergelijkbaar met convolutional connectiviteit geometrie, maar vooraf gedefinieerde functies aan de waarden van de bron wordt gebruikt voor het afleiden van de waarde van het knooppunt bestemming. Groepsgewijze verbindingen bundels hebben dus geen trainable staat (gewichten of systematische). Groepsgewijze verbindingen bundels ondersteuning voor de convolutional kenmerken, met uitzondering van **delen**, **MapCount**en **gewichten**.  

Normaal gesproken de kernels samengevat door aangrenzende groepsgewijze verbindingen eenheden elkaar niet overlappen. Als stap [d] gelijk aan [d] KernelShape in elke dimensie is, is de laag verkregen de traditionele lokale groepsgewijze verbindingen laag, die meestal wordt gebruikt in netwerken met convolutional neural. Elk bestemmingsknooppunt berekent de maximale of het gemiddelde van de activiteiten van de kernel in de bronlaag.  

In het volgende voorbeeld ziet u een bundel groepsgewijze verbindingen: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   De ariteit van de bundel is 3 (de lengte van de tupels, **InputShape**, **KernelShape**en **Stapsgewijze**). 
-   Het aantal knooppunten in de bronlaag is _5 *24* 24 2880 =_. 
-   Dit is een traditionele lokale groepsgewijze verbindingen laag omdat **KernelShape** en **Stapsgewijze** gelijk zijn. 
-   Het aantal knooppunten in de doellaag is _5 *12* 12 = 1440_.  
    
Raadpleeg de volgende artikelen voor meer informatie over groepsgewijze verbindingen lagen:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Zie punt 3.4)
-   [http://CS.Nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.Nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Antwoord normalisatie bundels
**Antwoord normalisatie** is een lokale normalisatie-schema dat is geïntroduceerd door Geoffrey Hinton, et al., in de papier- [ImageNet-Classiﬁcation met diepe Convolutional Neural Networks](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Normalisatie van de reactie wordt gebruikt bij generalisatie van neural netten. Wanneer een neuron wordt uitgevoerd op een van zeer hoog Activeringsniveau, wordt een lokale reactie normalisatie laag onderdrukt het Activeringsniveau van de omringende neurons. Dit gebeurt met behulp van drie parameters (***α***, ***β***en ***k***) en een convolutional-structuur (of een groep vorm). Elke neuron in de doellaag ***y*** correspondeert met een neuron ***x*** in de bronlaag. Het Activeringsniveau van ***y*** wordt gegeven door de volgende formule, waarbij ***f*** is het Activeringsniveau van een neuron en ***Nx*** is de kernel (of de set met de neurons in de buurt van ***x***), zoals gedefinieerd door de structuur van de volgende convolutional:  

![][1]  

Antwoord normalisatie bundels ondersteuning voor de convolutional kenmerken, met uitzondering van **delen**, **MapCount**en **gewichten**.  
 
-   Als de kernel neurons in de dezelfde kaart als ***x bevat***, is de regeling normalisatie wel **dezelfde kaart normalisatie**genoemd. Als u dezelfde kaart normalisatie definieert, moet de eerste coördinaat in **InputShape** de waarde 1 hebben.
-   Als de kernel neurons in de ruimtelijke positie van ***x bevat***, maar de neurons in andere kaarten zijn, wordt het schema voor normalisatie **over kaarten normalisatie**genoemd. Dit type antwoord normalisatie implementeert een vorm van laterale inhibitie geïnspireerd op het type gevonden in echte neurons, de concurrentie voor grote activering niveaus onder neuron output berekend op verschillende kaarten maken. Om aan te geven over normalisatie kaarten, moet de eerste coördinaat een geheel getal groter dan één en niet groter is dan het aantal kaarten, en de rest van de coördinaten de waarde 1 moet hebben.  

Omdat u een vooraf gedefinieerde functie antwoord normalisatie bundels toepassen op de waarden van de bron om te bepalen van de waarde van het knooppunt bestemming, hebben ze geen trainable staat (gewichten of systematische).   

**Waarschuwing**: de knooppunten in de doellaag komen overeen met neurons die de centrale knooppunten van de kernels zijn. Als KernelShape [d] is oneven, dan bijvoorbeeld _KernelShape [d] / 2_ komt overeen met de centrale kernel-knooppunt. Als _KernelShape [d]_ een even getal is, is het centrale knooppunt _KernelShape [d] / 2-1_. Daarom, als **Opvulling**[d] ONWAAR, de eerste en de laatste is _KernelShape [d] / 2_ knooppunten hebben geen overeenkomende knooppunten in de doellaag. Definiëren als u wilt voorkomen dat deze situatie, **Opvulling** als [true, true,..., true].  

Naast de vier kenmerken die eerder zijn beschreven, ondersteuning antwoord normalisatie bundels ook voor de volgende kenmerken:  

-   **Alpha**: (vereist) geeft een getal met drijvende komma-waarde die overeenkomt met ***α*** in de vorige formule. 
-   **Beta**: (vereist) geeft een getal met drijvende komma-waarde die overeenkomt met ***β*** in de vorige formule. 
-   **Offset**: (optioneel) geeft een getal met drijvende komma-waarde die overeenkomt met ***k*** in de vorige formule. 1 de standaardwaarde.  

In het volgende voorbeeld definieert een reactie normalisatie bundel met behulp van deze kenmerken:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   De bronlaag bevat vijf kaarten, elk met 12 x 12 in 1440 knooppunten Samentelling dimensie aof. 
-   De waarde van **KernelShape** geeft aan dat dit een dezelfde kaart normalisatie laag waarin de groep een rechthoek 3 x 3 is. 
-   De standaardwaarde van **Opvulling** is ingesteld op False, de doellaag is dus slechts 10 knooppunten in elke dimensie. Toevoegen om een knooppunt opnemen in de doellaag die overeenkomt met elk knooppunt in de bronlaag, opvulling = [true, true, true]; en de grootte van RN1 [5, 12, 12].  

## <a name="share-declaration"></a>Delen van de aangifte 
NET # ondersteunt eventueel meerdere bundels met gedeelde waarden te definiëren. Het gewicht van de twee bundels kunnen worden gedeeld als hun structuren hetzelfde zijn. Bundels met gedeelde gewichten Hiermee definieert u de volgende syntaxis:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

De volgende delen-declaratie bevat bijvoorbeeld de namen van de lagen die aangeeft dat systematische en gewichten worden gedeeld:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   De input functies ingedeeld in twee gelijke grootte invoer lagen. 
-   Verborgen lagen berekent u hogere functies op de twee lagen van de invoer. 
-   De share-verklaring geeft aan dat _H1_ en _H2_ op dezelfde manier uit hun respectieve ingangen moeten worden berekend.  
 
Ook dit kan worden met twee afzonderlijke delen verklaringen als volgt opgegeven:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

U kunt de korte vorm alleen als de lagen een enkel pakket bevatten. In het algemeen kan delen alleen als de desbetreffende structuur identiek zijn is, zodat ze dezelfde grootte, hetzelfde convolutional geometrie, enzovoort.  

## <a name="examples-of-net-usage"></a>Voorbeelden van gebruik Net #
Deze sectie bevat voorbeelden van hoe u Net # kunt verborgen lagen toevoegen, definiëren de manier waarop verborgen lagen interactie met andere lagen en convolutional netwerken.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Een eenvoudig netwerk met aangepaste neural definiëren: "Hello World" voorbeeld
Dit eenvoudige voorbeeld wordt het maken van een neural network-model met één laag verborgen.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Het voorbeeld van enkele basisopdrachten als volgt:  

-   De eerste regel definieert de invoer laag ( _gegevens_genoemd). Als u het sleutelwoord **auto** , de neural network alle kolommen van de functie automatisch opgenomen in de invoer voorbeelden. 
-   De tweede regel wordt gemaakt van de laag verborgen. De naam _H_ is toegewezen aan de verborgen laag die 200 knooppunten heeft. Deze laag is volledig verbonden met de input-laag.
-   De derde regel definieert de uitvoer laag (met de naam _O_), waarvan 10 uitvoer knooppunten bevat. Als het netwerk neural wordt gebruikt voor de indeling, is er één knooppunt van uitvoer per klasse. Het sleutelwoord **sigmoid** geeft aan dat de functie van de uitvoer wordt toegepast op de laag van de uitvoer.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definiëren van meerdere verborgen lagen: computer vision voorbeeld
In het volgende voorbeeld laat zien hoe een iets complexer neural netwerk met meerdere aangepaste verborgen lagen te definiëren.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

In dit voorbeeld ziet u verschillende functies van de taal van de specificatie neural networks:  

-   De structuur heeft twee input lagen _Pixels_ en _metagegevens_.
-   De _Pixels_ laag is een bronlaag voor twee bundels verbinding, met bestemming lagen, _ByRow_ en _ByCol_.
-   De lagen _verzamelen_ en het _resultaat_ zijn bestemming lagen in meerdere pakketten van de verbinding.
-   De laag van de output, _resultaat_, is een doellaag in twee pakketten van de verbinding; een met de tweede niveau verborgen (verzamelen) als een doellaag en de andere met de input-laag (metagegevens) als een laag bestemming.
-   Met expressies predikaat opgeven de verborgen lagen, _ByRow_ en _ByCol_, gefilterde connectiviteit. Meer precies, het knooppunt in de _ByRow_ op [x, y] is aangesloten op de knooppunten die gelijk is aan de eerste coördinaat van het knooppunt, de eerste index-coördinaat in _Pixels_ x. Ook het knooppunt in de _ByCol op [x, y] is aangesloten op de knooppunten in _Pixels_ met de tweede index coördineren binnen één van de tweede coördinaat van het knooppunt, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Een convolutional netwerk voor multiclass indeling definiëren: cijfer opname-voorbeeld
De definitie van het volgende netwerk is ontworpen voor het herkennen van getallen en ziet u enkele geavanceerde technieken voor het aanpassen van een neural network.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   De structuur heeft één invoer laag, _afbeelding_.
-   Het sleutelwoord **convolve** geeft aan dat de lagen met de naam _Conv1_ en _Conv2_ convolutional lagen. Elk van deze declaraties laag wordt gevolgd door een lijst met de kenmerken van het convolutiefilter.
-   Het net heeft een derde verborgen laag, _Hid3_, die volledig is aangesloten op de tweede verborgen laag _Conv2_.
-   De uitvoer-laag _cijfer_, wordt alleen gekoppeld aan de derde verborgen laag _Hid3_. Het sleutelwoord **alle** geeft aan dat de uitvoer laag volledig is verbonden met _Hid3_.
-   De ariteit van het convolutiefilter is drie (de lengte van de tupels, **InputShape**, **KernelShape**, **Stapsgewijze**en **delen**). 
-   Het aantal of gewicht per kernel is 1 _+ **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Of 26 * 50 = 1300_.
-   U kunt de knooppunten in elke laag verborgen als volgt berekenen:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   Het totale aantal knooppunten kan worden berekend met behulp van de gedeclareerde dimensionaliteit van de laag, [50, 5, 5] als volgt: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Omdat **delen**[d] ingesteld op False is voor _d == 0_, het aantal kernels is _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Bevestigingen

De netto #-taal voor het aanpassen van de architectuur van neural networks is bij Microsoft ontwikkeld door Shon Katzenberger (Architect, Machine Learning) en Alexey Kamenev (Software Engineer, Microsoft Research). Het wordt intern gebruikt voor de computer leren werken met projecten en toepassingen variërend van afbeelding detectie tot analytics tekst. Zie voor meer informatie, [Neural netten in Azure ML - Inleiding tot het Net #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
