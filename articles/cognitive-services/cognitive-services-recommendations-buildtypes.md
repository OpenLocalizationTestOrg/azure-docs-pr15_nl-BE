<properties
    pageTitle="Build-type en Model kwaliteit: aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen--quick start guide"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Type en model kwaliteit bouwen #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Ondersteunde build-typen ##

De aanbevelingen-API ondersteunt momenteel twee typen van de build: *aanbeveling* en *FBT*. Elk is gebouwd met verschillende algoritmen en elk heeft verschillende sterke punten. Dit document worden beschreven van deze opbouw en technieken voor het vergelijken van de kwaliteit van de modellen die zijn gegenereerd.

Als u nog niet gedaan hebt, raden wij u voltooit de [quick start guide](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Aanbeveling build type ###

Het type aanbeveling build gebruikt matrix factoriseren voor aanbevelingen. Deze [voorziening latente](https://en.wikipedia.org/wiki/Latent_variable) vectoren op basis van uw transacties voor het beschrijven van elk item wordt gegenereerd en gebruikt u vervolgens de latente vectoren vergelijken items zoals.

Als u het model op basis van de aankopen in uw winkel voor electronics trainen en een 650 Lumia telefoon als invoer voor het model bieden, resulteert het model in een reeks artikelen die meestal worden gekocht door mensen die waarschijnlijk een 650 Lumia telefoon kopen. De items zijn mogelijk niet complementair. In dit voorbeeld is het mogelijk dat het model andere telefoons retourneren zal omdat mensen die de Lumia 650 andere telefoons kan houden.

Het opbouwen van de aanbeveling heeft twee mogelijkheden die het aantrekkelijk maken:

*Koude item *ondersteunt de aanbeveling build ** plaatsing **

Items die geen significante gebruik, koude items worden genoemd. Als u een verzending van een telefoon die u nooit ontvangt vóór hebt verkocht, kan niet het systeem bijvoorbeeld aanbevelingen voor dit product alleen transacties afgeleid. Dit betekent dat het systeem van informatie over het product zelf moet leren.

Als u gebruik wilt maken koude artikelen worden geplaatst, moet u functies informatie voor elk van de items in de catalogus. Hieronder ziet u wat de eerste paar regels van de catalogus kunnen er als volgt uitzien (de sleutel = waarde opmaak van notitie voor de functies).

>6CX-00001, Surface Pro2, oppervlak,, Type Hardware, opslag = = 128 GB geheugen = 4G, fabrikant = Microsoft

>73H-00013, Wake Xbox 360 Gaming,, Type = Software taal Engels, score = = volwassen

>Bezwaar 0F05, Minecraft, Xbox 360, Gaming,, * Type Software, Language = = Spaans, score = jeugd

U moet ook de volgende build parameters instellen:

| Parameter maken         | Notities
|------------------     |-----------
|*useFeaturesInModel*     | Instellen op **true**.  Hiermee wordt aangegeven als functies kunnen worden gebruikt voor het verbeteren van het model van de aanbeveling.
|*allowColdItemPlacement*   | Instellen op **true**. Geeft aan dat als de aanbeveling moet ook push-koude items via de functie vergelijkbaar.
| *modelingFeatureList*   | Door komma's gescheiden lijst met onderdelen om te worden gebruikt bij het opbouwen van de aanbeveling voor het verbeteren van de aanbeveling. Bijvoorbeeld, "taal, opslag" in het voorgaande voorbeeld.

**De aanbeveling build ondersteunt de aanbevelingen van de gebruiker**

Een aanbeveling build ondersteunt de [aanbevelingen van de gebruiker](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Dit betekent dat persoonlijke aanbevelingen voor gebruikers op basis van hun transactie geschiedenis kan bieden. Voor de aanbevelingen van de gebruiker, kan u de gebruikers-ID of de recente historie van transacties voor die gebruiker opgeven.

Een klassiek voorbeeld van de plaats waar u mogelijk toe te passen aanbevelingen van de gebruiker bij het aanmelden op de pagina Welkom is. U kunt er inhoud die van toepassing op de specifieke gebruiker is promoveren.

U kunt ook een type aanbevelingen build van toepassing wanneer de gebruiker is uitgecheckt. Op dat moment hebt u de lijst met items die de gebruiker wil kopen en u kunt geven aanbevelingen op basis van de huidige markt mand.

#### <a name="recommendations-build-parameters"></a>Aanbevelingen maken parameters

| Naam  |   Beschrijving |    Type, <br>  geldige waarden <br> (standaardwaarde)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Het aantal iteraties het model voert komt tot uiting in de algehele compute-tijd en de nauwkeurigheid van het model. Hoe hoger het getal, des te nauwkeuriger het model, maar de tijd berekenen duurt langer.  |   Integer, <br>  10 tot 50 <br>(40)
| *NumberOfModelDimensions* |   Het aantal dimensies in relatie tot het aantal functies die het model proberen zal te vinden met uw gegevens. Het aantal dimensies verhoogt kan beter "fine-tuning"-van de resultaten in kleinere clusters. Te veel dimensies kunnen echter het model niet vinden de correlaties tussen de items. |  Integer, <br> 10-40 <br>(20) |
| *ItemCutOffLowerBound* |  Het minimum aantal punten van gebruik van die een artikel zou moeten hebben om te worden beschouwd als onderdeel van het model worden gedefinieerd. |        Integer, <br> 2 of meer <br> (2) |
| *ItemCutOffUpperBound* |  Definieert het maximum aantal punten van gebruik van die een artikel zou moeten hebben om te worden beschouwd als onderdeel van het model. |  Integer, <br>2 of meer<br> (2147483647) |
|*UserCutOffLowerBound* |   Het minimum aantal transacties die moet een gebruiker uitgevoerd om te worden beschouwd als onderdeel van het model worden gedefinieerd. | Integer, <br> 2 of meer <br> (2)
| *UserCutOffUpperBound* |  Definieert het maximum aantal transacties die moet een gebruiker uitgevoerd om te worden beschouwd als onderdeel van het model. | Integer, <br>2 of meer <br> (2147483647)|
| *UseFeaturesInModel* |    Hiermee wordt aangegeven als functies kunnen worden gebruikt voor het verbeteren van het model van de aanbeveling. |     Boole-waarde<br> Default: True
|*ModelingFeatureList* |    Door komma's gescheiden lijst met onderdelen om te worden gebruikt bij het opbouwen van de aanbeveling voor het verbeteren van de aanbeveling. Dit is afhankelijk van de functies die belangrijk zijn. |    Een tekenreeks, maximaal 512 tekens.
| *AllowColdItemPlacement* |    Geeft aan dat als de aanbeveling moet ook push-koude items via de functie vergelijkbaar. | Boole-waarde <br> Default: False
| *EnableFeatureCorrelation*    | Hiermee wordt aangegeven als functies kunnen worden gebruikt in het redeneren. | Boole-waarde <br> Default: False
| *ReasoningFeatureList* |  Door komma's gescheiden lijst met onderdelen moet worden gebruikt voor zinnen, zoals de aanbeveling uitleg redeneren. Dit is afhankelijk van de functies die belangrijk voor klanten zijn. | Een tekenreeks, maximaal 512 tekens.
| *EnableU2I* | Persoonlijke aanbevelingen, ook wel genoemd gebruiker artikel (U2I) aanbevelingen inschakelen. | Boole-waarde <br>Default: True
|*EnableModelingInsights* | Hiermee definieert u of off line evaluatie verzamelen inzichten modellering (precision en diversiteit-metrics) moet worden uitgevoerd. Als is ingesteld op true, wordt een subset van de gegevens zullen niet worden gebruikt voor training, omdat deze worden gereserveerd moet voor het testen van het model. Meer informatie over [off line beoordelingen](#OfflineEvaluation). | Boole-waarde <br> Default: False
| *SplitterStrategy* | Als enable modeling inzichten is ingesteld op *true*, is dit hoe gegevens moeten worden verdeeld voor evaluatiedoeleinden.  | String, *RandomSplitter* of *LastEventSplitter* <br>Standaard: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>Type van de gecompileerde versie FBT ###

De vaak gekocht bij elkaar (FBT) build is een analyse die telt het aantal keren dat twee of drie verschillende producten samen worden opgeteld. Het wordt op basis van een vergelijkbaar functie (**collega exemplaren**, **Jaccard**, **tilt**) vervolgens gesorteerd.

Zie **Jaccard** en **til** als manieren om te normaliseren collega exemplaren.  Dit betekent dat de artikelen worden geretourneerd als deze waar aangeschaft samen met het item zaad.

In ons voorbeeld 650 Lumia telefoon wordt telefoon X geretourneerd als telefoon X in dezelfde sessie als de telefoon Lumia 650 is gekocht. Omdat dit misschien onwaarschijnlijk, zou verwachten we artikelen aanvulling vormen op de 650 Lumia moet worden gegeven; bijvoorbeeld een schermbeveiliging of een adapter voor de Lumia 650.

Op dit moment worden twee items verondersteld te kopen in dezelfde sessie die zich voordoen bij een transactie met dezelfde gebruikersnaam en tijdstempel.

FBT builds ondersteunen geen koude items, omdat per definitie verwacht twee items dat te kopen in dezelfde transactie. Terwijl het FBT builds kunnen terugkeren van sets van artikelen (groepen van drie cijfers), ondersteunen geen persoonlijke aanbevelingen omdat ze een zaad één item als invoer accepteren.


#### <a name="fbt-build-parameters"></a>FBT build parameters

| Naam  |   Beschrijving |       Type,  <br> geldige waarden <br> (standaardwaarde)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Hoe conservatieve model is. Aantal collega exemplaren van artikelen die moeten worden overwogen voor modellering. |  Integer, <br> 3-50 <br> (6)
| *FbtMaxItemSetSize* | Het aantal items in een veelgebruikte set bounds.| Geheel getal  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimale score die een veelgebruikte set moet worden opgenomen in de geretourneerde resultaten moet hebben. Hoe hoger hoe beter. | Getal met dubbele precisie <br> 0 en hoger <br> (0)
| *FbtSimilarityFunction* | Definieert de functie vergelijkbaar zijn voor de bouw. **Til** geeft voorrang aan serendipity, **collega exemplaar** geeft voorrang aan voorspelbaarheid en **Jaccard** is een compromis tussen de twee. | String,  <br>  <i>cooccurrence, liften, jaccard</i><br> Standaard: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Evaluatie en selectie maken ##

Deze richtlijnen waarmee u kunt bepalen of u een build van aanbevelingen of een versie FBT moet gebruiken, maar biedt een sluitend antwoord in gevallen waarin u een van deze. Ook, zelfs als u weet dat u wilt gebruiken een versie FBT, u mogelijk toch **Jaccard** of **optillen** als de overeenkomstige functie te kiezen.

De beste manier om te kiezen tussen twee verschillende versies is te testen in de echte wereld (on line evaluatie) en een omrekeningskoers voor de verschillende versies bijhouden. De omrekeningskoers kan worden gemeten op basis van de aanbeveling klikken, het werkelijke aantal aankopen van aanbevelingen weergegeven, of zelfs op de werkelijke aankoopbedragen wanneer de verschillende aanbevelingen zijn weergegeven. U mag uw conversie snelheid metric op basis van de doelstelling van uw bedrijf.

In sommige gevallen kunt u offline het model te evalueren voordat u deze in de productie plaatsen. Off line evaluatie is geen vervanging voor on line evaluatie, kan het dienen als een metric.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Off line evaluatie  ##

Het doel van een off line evaluatie is om te voorspellen precisie (het aantal gebruikers dat een van de aanbevolen items te kopen) en de diversiteit van de aanbevelingen (het aantal items die worden aanbevolen).
Als onderdeel van de evaluatie van de cijfers precisie en diversiteit, het systeem vindt u een voorbeeld van de gebruikers en de transacties voor gebruikers in twee groepen gesplitst: de dataset training en de dataset test.

> [AZURE.NOTE]Voor het gebruik van off line gegevens hebt u tijdstempels in uw gebruiksgegevens.
> Tijd is vereist voor gebruik tussen opleidings- en datasets correct splitsen.

> Off line evaluatie kan ook geen resultaten voor het gebruik van kleine bestanden opleveren. Voor de evaluatie moet worden uitgebreid, moet er minimaal 1000 punten voor gebruik in de dataset test.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Precisie in k ###
De volgende tabel geeft de uitvoer van de off line precisie op k-evaluatie.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Percentage |   13,75 | 18.04   | 21 |  24.31 | 26.61
|Gebruikers in de test |    10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Gebruikers die worden beschouwd als | 10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Gebruikers niet beschouwd als | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
In de voorgaande tabel *k* het nummer voorstelt van aanbevelingen aan de klant wordt weergegeven. De tabel wordt als volgt gelezen: "Als tijdens de testperiode is slechts een aanbeveling aan de klanten wordt weergegeven, alleen 13,75 van de gebruikers zou hebben gekocht die aanbeveling." Deze verklaring is gebaseerd op de veronderstelling dat het model met de inkoopgegevens is opgeleid. Een andere manier om te zeggen dit is dat de precisie op 1 13,75.

Ziet u dat als u meer items aan de klant worden weergegeven, de waarschijnlijkheid dat de klant een aanbevolen object omhoog gaat. Voor de voorgaande experiment verdubbeld de kans bijna tot 26.61 procent wanneer 5 items worden aanbevolen.

#### <a name="percentage"></a>Percentage
Het percentage van de gebruikers die interactief gecommuniceerd met ten minste één van de aanbevelingen van *k* wordt weergegeven. Het percentage wordt berekend door het aantal gebruikers die interactief gecommuniceerd met ten minste één aanbeveling door het totale aantal gebruikers beschouwd als delen. Zie voor meer informatie als gebruikers.

#### <a name="users-in-test"></a>Gebruikers in de test
Gegevens in deze rij geeft het totale aantal gebruikers in de dataset test.

#### <a name="users-considered"></a>Gebruikers die worden beschouwd als
Een gebruiker wordt alleen beschouwd als het systeem ten minste aanbevolen *k* op basis van het model dat is gegenereerd met behulp van de dataset training.

#### <a name="users-not-considered"></a>Gebruikers niet beschouwd als
Gegevens in deze rij vertegenwoordigt alle gebruikers niet beschouwd. De gebruikers die niet ten minste hebben ontvangen *k* aanbevolen onderdelen.

Gebruiker niet beschouwd als gebruikers in de test: = gebruikers beschouwd als

<a name="Diversity"></a>
### <a name="diversity"></a>Diversiteit ###
Het type items aanbevolen diversiteit metrics te meten. De volgende tabel geeft de uitvoer van de off line evaluatie van diversiteit.

|Bucket percentiel |    0-90|  90 99| 99-100
|------------------|--------|-------|---------
|Percentage        | 34.258 | 55.127| 10.615


Totaal aanbevolen items: 100.000

Unieke items aanbevolen: 954

#### <a name="percentile-buckets"></a>Buckets percentiel
Elke bucket percentiel wordt voorgesteld door een reeks (minimum en maximum waarden die tussen 0 en 100 liggen). Bijna 100 artikelen zijn de meest populaire artikelen en de artikelen dicht bij 0 het minst populair zijn. Bijvoorbeeld, als de percentagewaarde voor de bucket 99-100-percentiel 10.6, betekent dit dat 10,6 procent van de aanbevelingen geretourneerd de bovenste één procent meest populaire artikelen. De minimumwaarde van percentiel bucket inclusief is en de maximale waarde is exclusief, met uitzondering van 100.
#### <a name="unique-items-recommended"></a>Unieke items die worden aanbevolen
De unieke artikelen aanbevolen metric bevat het nummer van de afzonderlijke artikelen die zijn geretourneerd voor een evaluatie.
#### <a name="total-items-recommended"></a>Totaal artikelen aanbevolen
Totaal artikelen aanbevolen metric geeft het aantal artikelen aan te raden. Sommige mogelijk dubbele records.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Off line evaluatie metrics ###
De precisie en diversiteit offline metrics kan nuttig zijn wanneer u selecteert welke versie moet worden gebruikt. Tijdens het bouwen als onderdeel van de respectieve FBT of aanbeveling bouwt parameters:

-   De parameter *enableModelingInsights* build ingesteld op **true**.
-   Selecteer de *splitterStrategy* (of *RandomSplitter* of *LastEventSplitter*).
*RandomSplitter* de gebruiksgegevens in trein wordt gesplitst en test wordt ingesteld op basis van het percentage van de test bepaald *randomSplitterParameters* en willekeurige waarden zaad.
*LastEventSplitter* de gebruiksgegevens in trein wordt gesplitst en wordt op basis van de laatste transactie voor elke gebruiker te testen.

Dit moet een gecompileerde versie die u gebruikt alleen een subset van de gegevens voor de opleiding en de rest van de gegevens gebruikt voor het berekenen van statistieken voor evaluatie worden geactiveerd.  Nadat het opbouwen is voltooid, moet de uitvoer van de evaluatie, u de [Get build metrics API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), doorgeven van de respectieve *model* en *buildId*aanroepen.

 Hieronder vindt u de JSON-uitvoer voor de evaluatie van het monster.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
