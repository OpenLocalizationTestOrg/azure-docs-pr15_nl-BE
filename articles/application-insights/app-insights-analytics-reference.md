<properties 
    pageTitle="Verwijzing in Analytics in inzichten toepassing | Microsoft Azure" 
    description="Verwijzing voor overzichten in Analytics, de krachtige zoekfunctie van inzichten van toepassing. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Verwijzing voor Analytics

[Analytics](app-insights-analytics.md) is de krachtige zoekfunctie van [Inzichten van toepassing](app-insights-overview.md). Deze pagina's beschrijven de querytaal Analytics.

> [AZURE.NOTE] [Test drive Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo) als uw app is niet van gegevens naar inzichten toepassing nog verzenden.

## <a name="index"></a>Index


**Laat** [laat](#let-clause)


**Query's en operators** [aantal](#count-operator)  |  [evalueren](#evaluate-operator) | [uitbreiden](#extend-operator) | [join](#join-operator) | [limiet](#limit-operator) | [mvexpand](#mvexpand-operator) | [parseren](#parse-operator) | [project](#project-operator) | [opslag project](#project-away-operator) | [bereik](#range-operator) | [verminderen](#reduce-operator) | [renderen van Richtlijn](#render-directive) | [component beperken](#restrict-clause) | [sorteren](#sort-operator) | [vatten](#summarize-operator) | [nemen](#take-operator) | [boven](#top-operator) | [boven geneste](#top-nested-operator) | [Unie](#union-operator) | [waar](#where-operator) | [waar in](#where-in-operator)

**Samenvoegingen** [alle](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [het aantal](#count) | [aantal](#countif) | [dcount](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [percentiel](#percentile) | [percentielen](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [stdev](#stdev) | [som](#sum) | [afwijking](#variance)

**Hoeken** [Letterlijke Booleaanse](#boolean-literals)  |  [Boole-operators](#boolean-operators) | [webcasts](#casts) | [scalaire vergelijkingen](#scalar-comparisons) | [gettype](#gettype) | [hash](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Nummers** [Rekenkundige operatoren](#arithmetic-operators)  |  [Numerieke letterlijke waarden](#numeric-literals) | [abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) | [log](#log) | [ASELECT](#rand) | [WORTEL](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Datum en tijd** [Datum en tijd expressies](#date-and-time-expressions)  |  [Datum- en tijdwaarden](#date-and-time-literals) | [geleden](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dag van de week](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [nu](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Tekenreeks** [GUID 's](#guids)  |  [Afgeschermd letterlijke](#obfuscated-string-literals) | [Letterlijke tekenreeks](#string-literals) | [tekenreeksvergelijkingen](#string-comparisons) | [countof](#countof) | [extract](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [vervangen door](#replace) | [splitsen](#split) | [strcat](#strcat) | [strlen](#strlen) | [subtekenreeks](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Arrays, objecten en dynamische** [Letterlijke tekens voor arrays en objecten](#array-and-object-literals)  |  [Dynamisch objectfuncties](#dynamic-object-functions) | [dynamische objecten in componenten laat](#dynamic-objects-in-let-clauses) | [JSON pad expressies](#json-path-expressions) | [namen](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [range](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Laat

### <a name="let-clause"></a>Laat de component

**In tabelvorm kunt - naam van een tabel**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Scalaire kunt - naam van een**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda laat - benoemen van een functie**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Een laat-component wordt een [naam](#names) aan een resultaat in tabelvorm, een scalaire waarde of een functie. De component is een voorvoegsel aan een query en de omvang van de binding die query is. (Let niet kunt u naam dingen die u later in de sessie gebruikt.)

**Syntaxis**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *type:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Een query niet wordt voorafgegaan door een laat-component.

**Voorbeelden**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Self-join:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Query's en operators

Een query op de telemetrie bestaat uit een verwijzing naar een bronstroom, gevolgd door een pijpleiding van filters. Bijvoorbeeld:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Elk filter dat wordt voorafgegaan door het teken `|` is een exemplaar van een *exploitant*, met enkele parameters. De invoer voor de operator is de tabel die het resultaat van de voorgaande pijpleiding. In de meeste gevallen zijn alle parameters [scalaire expressies](#scalars) over de kolommen van de invoer. De parameters zijn de namen van kolommen invoer in enkele gevallen en in enkele gevallen is de parameter een tweede tabel. Het resultaat van een query is altijd een tabel, zelfs als deze slechts één kolom en één rij.

Query's kunnen afzonderlijke regeleinden bevatten, maar wordt afgesloten met een lege regel. Zij kunnen bevatten opmerkingen tussen `//` en het einde van de regel.

Een query kan worden voorafgegaan door een of meer [componenten te laten](#let-clause), die hoeken, tabellen of functies die kunnen worden gebruikt in de query definiëren.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`in onderstaande voorbeelden van query wordt gebruikt ter aanduiding van de vorige tabel voor pijpleiding of bron.
> 

### <a name="count-operator"></a>de operator Count

De `count` operator geeft als resultaat het aantal records (rijen) in de recordreeks invoer.

**Syntaxis**

    T | count

**Argumenten**

* *T*: de gegevens in tabelvorm waarvan de records worden geteld.

**Geeft als resultaat**

Deze functie resulteert in een tabel met een enkele record en kolom van het type `long`. De waarde van de cel alleen is het aantal records in *T*. 

**Voorbeeld**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>operator evalueren

`evaluate`is een mechanisme voor de extensie waarmee gespecialiseerde algoritmen dat wordt toegevoegd aan query's.

`evaluate`de laatste in de pijplijn query-operator moet worden (met uitzondering van een mogelijk `render`). Het mag niet in de hoofdtekst van een functie.

[evalueren van autocluster](#evaluate-autocluster) | [evalueren mandje](#evaluate-basket) | [evalueren diffpatterns](#evaluate-diffpatterns) | [extractcolumns evalueren](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>autocluster evalueren

     T | evaluate autocluster()

AutoCluster algemene patronen van individuele kenmerken (dimensies) zoekt in de gegevens en de resultaten van de oorspronkelijke query (of het gaat om 100 of 100 k rijen) wordt beperkt tot een klein aantal patronen. AutoCluster is ontwikkeld om u te helpen bij het analyseren van mislukte pogingen (b.v. uitzonderingen, crashes), maar mogelijk op een gefilterde set met gegevens kunnen werken. 

**Syntaxis**

    T | evaluate autocluster( arguments )

**Geeft als resultaat**

AutoCluster retourneert een (meestal kleine) patronen die gedeelten van de gegevens met gedeelde gemeenschappelijke waarden via meerdere, afzonderlijke kenmerken vastleggen. Elk patroon wordt vertegenwoordigd door een rij in de resultaten. 

De eerste twee kolommen zijn het aantal en het percentage van de oorspronkelijke query rijen die zijn vastgelegd door het patroon. De overige kolommen uit de oorspronkelijke query en hun waarde is een bepaalde waarde uit de kolom of ' *' wat betekent dat de waarden van variabelen. 

De patronen zijn niet gescheiden: ze kunnen worden overlappende en meestal omvatten niet de oorspronkelijke rijen. Enkele rijen kunnen niet vallen onder een patroon.

**Tips**

* Gebruik `where` en `project` in de pipe invoer om te beperken de gegevens net wat u zoekt.
* Wanneer u een rij interessant vindt, wilt u mogelijk lager verder door de specifieke waarden aan toe te voegen uw `where` filter.

**Argumenten (optioneel)**

* `output=all | values | minimal` 

    De indeling van de resultaten. Het aantal en het percentage kolommen worden altijd weergegeven in de resultaten. 

 * `all`-alle kolommen van de invoer worden uitgevoerd
 * `values`-kolommen met alleen gefilterd op ' *' in de resultaten
 * `minimal`-kolommen die identiek voor alle rijen in de oorspronkelijke query zijn ook gefilterd. 


* `min_percent=`*dubbel* (standaard: 1)

    De minimumwaarde voor het percentage dekking van de gegenereerde rijen.

    Voorbeeld:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (standaardwaarde: 25) 

    Het aantal zaden bepaalt het aantal punten van het algoritme voor lokale zoekt. In sommige gevallen, afhankelijk van de structuur van de gegevens, verhoogt het aantal zaden het nummer (of kwaliteit) van de resultaten door toegenomen zoeken ruimte aan trage query verhouding. Het argument num_seeds heeft de resultaten in beide richtingen verminderd zodat het afnemende onder 5 te verwaarlozen prestatieverbeteringen bereiken en verhogen boven 50 zelden meer patronen genereert.

    Voorbeeld:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < dubbele < 1*+ (standaard: 0,5)

    Hebt u enige controle over het evenwicht tussen algemene (hoge dekking) en informatieve (veel gedeelde waarden). Toenemende size_weight vermindert gewoonlijk het aantal patronen en elk patroon krijgt een groter percentage te dekken. Meestal size_weight verlagen produceert meer specifieke patronen met meer gedeelde waarden en een kleiner percentage dekking. De onder de motorkap is de formule een gewogen meetkundig gemiddelde tussen de gestandaardiseerde score van algemene en informatieve score met size_weight en 1 size_weight als de gewichten. 

    Voorbeeld:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*kolomnaam*

    Elke rij in de invoer op basis van het opgegeven gewicht acht (standaard heeft elke rij een gewicht van '1'), is het gemeenschappelijk gebruik van een kolom gewicht houden rekening bemonstering of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    Voorbeeld:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>mandje evalueren

     T | evaluate basket()

Mandje wordt gezocht naar alle regelmatige patronen van individuele kenmerken (dimensies) in de gegevens en alle voorkomende patronen die de drempel frequentie in de oorspronkelijke query doorgegeven wordt geretourneerd. Mandje zoeken naar alle voorkomende patronen in de gegevens wordt gegarandeerd, maar wordt niet gegarandeerd polynomiale runtime. De uitvoering van de query lineair is in het aantal rijen, maar in sommige gevallen mogelijk exponentiële in het aantal kolommen (dimensies). Mandje is gebaseerd op de oorspronkelijk ontwikkeld voor mandje analyse datamining Apriori-algoritme. 

**Geeft als resultaat**

Alle patronen die in meer dan een opgegeven fractie (standaard 0,05) van de gebeurtenissen.

**Argumenten (optioneel)**


* `threshold=`*0.015 < dubbele < 1* (standaard: 0,05) 

    Hiermee stelt u de minimale verhouding van de rijen moeten regelmatig worden beschouwd (patronen met kleinere ratio niet wordt geretourneerd).

    Voorbeeld:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*kolomnaam*

    Elke rij in de invoer op basis van het opgegeven gewicht acht (standaard heeft elke rij een gewicht van '1'), is het gemeenschappelijk gebruik van een kolom gewicht houden rekening bemonstering of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    Voorbeeld: T | basket("weight_column=sample_Count") evalueren


* `max_dims=`*1 < int* (standaard: 5)

    Het maximale aantal niet-gerelateerde dimensies wordt per mand, standaard om de uitvoering van de query wordt beperkt.


* `output=minimize` | `all` 

    De indeling van de resultaten. Het aantal en het percentage kolommen worden altijd weergegeven in de resultaten.

 * `minimize`-kolommen met alleen gefilterd op ' *' in de resultaten.
 * `all`-alle kolommen van de invoer worden uitgevoerd.




#### <a name="evaluate-diffpatterns"></a>diffpatterns evalueren

     requests | evaluate diffpatterns("split=success")

Diffpatterns vergelijkt twee gegevenssets van dezelfde structuur en de patronen van individuele kenmerken (dimensies) dat de verschillen tussen de twee gegevensverzamelingen kenmerkt. Diffpatterns is ontwikkeld om u te helpen bij het analyseren van mislukte pogingen (bijvoorbeeld door fouten vergelijken op storingen in een bepaalde periode), maar mogelijk verschillen tussen de twee sets gegevens van dezelfde structuur kunt vinden. 

**Syntaxis**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Geeft als resultaat**

Diffpatterns retourneert een (meestal kleine) patronen die verschillende delen van de gegevens in twee verzamelingen (dat wil zeggen een patroon voor het vastleggen van een groot percentage van de rijen in de eerste gegevensverzameling en laag percentage van de rijen in de tweede set). Elk patroon wordt vertegenwoordigd door een rij in de resultaten.

De eerste vier kolommen worden het aantal en het percentage van de oorspronkelijke query rijen die zijn vastgelegd door het patroon in elke set, de vijfde kolom is het verschil (in absolute procentpunten) tussen de twee reeksen. De overige kolommen uit de oorspronkelijke query en hun waarde is een bepaalde waarde uit de kolom of * wat betekent dat de waarden van variabelen. 

De patronen zijn niet onderscheiden: ze kunnen worden overlappende en meestal omvatten niet de oorspronkelijke rijen. Enkele rijen kunnen niet vallen onder een patroon.

**Tips**

* Waar gebruikt en in de pipe invoer om te beperken de gegevens gewoon wat u geïnteresseerd bent in project.

* Wanneer u een rij interessant vindt, kunt u detailinformatie verder door de specifieke waarden voor het filteren van uw locatie toe te voegen.

**Argumenten**

* `split=`*naam van de kolom* (vereist)

    De kolom moet precies twee waarden hebben. Maak indien nodig een dergelijke kolom:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*tekenreeks*

    Geeft het algoritme waarmee wordt alleen gezocht naar patronen die een hoger percentage hebben in de doelgegevensset, het doel moet een van de twee waarden van de kolom splitsen.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < dubbele < 1* (standaard: 0,05) 

    Hiermee stelt u de minimale patroon (verhouding) verschil tussen de twee sets.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    De indeling van de resultaten. Het aantal en het percentage kolommen worden altijd weergegeven in de resultaten. 

 * `minimize`-kolommen met alleen gefilterd op ' *' in de resultaten
 * `all`-alle kolommen van de invoer worden uitgevoerd

* `weight_column=`*kolomnaam*

    Elke rij in de invoer op basis van het opgegeven gewicht acht (elke rij heeft standaard een gewicht van '1'). Een gemeenschappelijk gebruik van een weging is in de bemonstering rekening of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>extractcolumns evalueren

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns wordt gebruikt om een tabel met meerdere eenvoudig kolommen dynamisch uitgepakte uit de (semi) gestructureerde kolom(men) op basis van hun type verrijken. Het ondersteunt momenteel json alleen kolommen, zowel dynamische en string serialisatie van jsons.


* `max_columns=`*int* (standaardwaarde: 10) 

    Het aantal nieuw toegevoegde kolommen is dynamisch en kan zeer groot (eigenlijk is het aantal verschillende sleutels in alle records van json) zodat we het moeten beperken. Nieuwe kolommen worden gesorteerd in aflopende volgorde op basis van de frequentie en tot max_columns aan de tabel toegevoegd.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*dubbel* (standaard: 10.0) 

    Een andere manier om nieuwe kolommen beperken door kolommen waarvan de frequentie lager dan min_percent is.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*BOOL* (standaard: true) 

    Als de waarde true wordt de naam van de complexe kolom als voorvoegsel toegevoegd aan de namen van kolommen uitgepakte.


* `prefix_delimiter=`*tekenreeks* (standaard: '_') 

    Als add_prefix = true deze parameter bepaalt het scheidingsteken dat wordt gebruikt om de namen van de nieuwe kolommen samen te voegen.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*BOOL* (standaard: false) 

    Als de waarde true wordt de oorspronkelijke (json) kolommen worden bewaard in de uitvoertabel.


* `output=query | table` 

    De indeling van de resultaten. 

 * `table`-De uitvoer is dezelfde tabel als minus de opgegeven invoer kolommen plus nieuwe kolommen die zijn opgehaald uit de kolommen invoer ontvangen.
 * `query`-De uitvoer is een tekenreeks die de query brengt u het resultaat als tabel. 




### <a name="extend-operator"></a>operator uitbreiden

     T | extend duration = stopTime - startTime

Een of meer berekende kolommen toevoegen aan een tabel. 


**Syntaxis**

    T | extend ColumnName = Expression [, ...]

**Argumenten**

* *T:* De invoer tabel.
* *ColumnName:* De naam van een kolommen toe te voegen. [Namen](#names) zijn hoofdlettergevoelig en mag alfabetische, numerieke of '_' tekens. Gebruik `['...']` of `["..."]` citeren trefwoorden of namen met andere tekens.
* *Expressie:* Een berekening via de bestaande kolommen.

**Geeft als resultaat**

Een kopie van de tabel invoer met de opgegeven extra kolommen.

**Tips**

* Gebruik [`project`](#project-operator) in plaats daarvan als u ook wilt verwijderen of hernoemen van sommige kolommen.
* Gebruik geen `extend` gewoon om een kortere naam wilt gebruiken in een long-expressie. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    De oorspronkelijke kolommen van de tabel is geïndexeerd; de naam van uw nieuwe definieert een extra kolom die niet is geïndexeerd, zodat de query is waarschijnlijk langzamer.

**Voorbeeld**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>join-operator

    Table1 | join (Table2) on CommonColumn

De rijen van twee tabellen samengevoegd door de overeenkomende waarden van de opgegeven kolom.


**Syntaxis**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumenten**

* *Tabel1* - 'links zijde"van de join.
* *Tabel2* - 'aan de rechterkant' van de join. Het kan zijn dat een geneste query-expressie die een tabel oplevert.
* *CommonColumn* - een kolom met dezelfde naam in de twee tabellen.
* *Type* - geeft aan hoe de rijen uit twee tabellen worden vergeleken.

**Geeft als resultaat**

Een tabel met:

* Een kolom voor elke kolom in elk van de twee tabellen, met inbegrip van de overeenkomstige sleutels. De kolommen aan de rechterkant van de naam wordt automatisch gewijzigd als naam is strijdig.
* Een rij voor elke overeenkomst tussen de invoertabellen. Een overeenkomst is een rij uit de ene tabel die dezelfde waarde voor alle heeft geselecteerde de `on` velden als een rij in de andere tabel. 

* `Kind`niet opgegeven

    Slechts één rij aan de linkerkant is afgestemd voor elke waarde van de `on` sleutel. De uitvoer bevat een rij voor elke overeenkomst van deze rij met rijen vanaf de rechterkant.

* `Kind=inner`
 
     Er is een rij in de uitvoer voor elke combinatie van overeenkomende rijen van links en rechts.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Naast de binnenste is er een rij voor elke rij de links (en/of rechts), zelfs als er geen overeenkomst. In dat geval wordt bevatten de uitvoer van niet-overeenkomende cellen null-waarden.

* `kind=leftanti`

     Geeft als resultaat alle records vanaf de linkerkant die geen overeenkomsten vanaf de rechterkant. De resultaattabel heeft alleen de kolommen aan de linkerkant. 
 
Als er meerdere rijen met dezelfde waarden voor deze velden, krijgt u rijen voor alle combinaties.

**Tips**

Voor de beste prestaties:

* Gebruik `where` en `project` ter vermindering van het aantal rijen en kolommen in de invoertabellen voordat de `join`. 
* Als een tabel altijd kleiner dan de andere is, kunt u deze als (piped) links in de join gebruiken.
* De kolommen in de join-overeenkomst moeten dezelfde naam hebben. Gebruik de operator project zo nodig de naam van een kolom in een van de tabellen wijzigen.

**Voorbeeld**

Activiteiten ophalen uit een logboek waarop bepaalde vermeldingen markeren het begin en einde van een activiteit worden uitgebreid. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>limiet-operator

     T | limit 5

Met deze eigenschap wordt het opgegeven aantal rijen uit de tabel invoer. Er is geen garantie welke records worden geretourneerd. (Als bepaalde records, gebruikt u [`top`](#top-operator).)

**Alias**`take`

**Syntaxis**

    T | limit NumberOfRows


**Tips**

`Take`is een eenvoudige en efficiënte manier om er een voorbeeld van de resultaten wanneer u interactief wilt werken. Let erop dat het geen garantie voor de productie van bepaalde rijen of ze in een bepaalde volgorde te produceren.

Er is een impliciete beperking van het aantal rijen dat wordt geretourneerd naar de client, zelfs als u geen `take`. Om deze limiet optillen, gebruikt u de `notruncation` optie voor het verzoek.



### <a name="mvexpand-operator"></a>mvexpand operator

    T | mvexpand listColumn 

Uitbreiding van een lijst in een cel hebt getypt van een dynamische (JSON) elke vermelding heeft een aparte rij. Alle andere cellen in een rij uitgevouwen worden gedupliceerd. 

(Zie ook [`summarize makelist`](#summarize-operator) die het tegengestelde functie wordt uitgevoerd.)

**Voorbeeld**

Stel dat de invoertabel wordt:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hallo"|{"sleutel": "waarde"}|
|2|"world"|[0,1, 'k', 'v']|

    mvexpand D

Resultaat is:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hallo"|{"sleutel": "waarde"}|
|2|"world"|0|
|2|"world"|1|
|2|"world"|"k"|
|2|"world"|"v"|


**Syntaxis**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumenten**

* *ColumnName:* Matrices in de kolom met de naam wordt in het resultaat uitgebreid naar meerdere rijen. 
* *ArrayExpression:* Een expressie die een matrix plaats maakt. Dit formulier wordt gebruikt, wordt een nieuwe kolom toegevoegd als bestaande behouden blijft.
* *Naam:* Een naam voor de nieuwe kolom.
* *Typename:* De uitgebreide uitdrukking aan een bepaald type wordt geworpen.
* *Gedurende:* Het maximum aantal rijen is gegenereerd op basis van de oorspronkelijke rij. De standaardwaarde is 128.

**Geeft als resultaat**

Meerdere rijen voor elk van de waarden in een matrix in de kolom met de naam of in de expressie van de matrix.

De kolom uitgevouwen heeft altijd dynamisch type. Cast gebruiken, zoals `todatetime()` of `toint()` als u wilt berekenen of de cumulatieve waarde.

Twee modi van de eigenschap zak uitbreidingen worden ondersteund:

* `bagexpansion=bag`: Eigenschap zakken zijn in zakken eigenschap één post uitgevouwen. Dit is de standaard expansie.
* `bagexpansion=array`: Eigenschap zakken in twee elementen worden uitgevouwen `[` *sleutel*`,`*waarde* `]` array van structuren, uniforme toegang tot de sleutels en waarden (evenals, bijvoorbeeld een afzonderlijke telling aggregatie berijden van namen). 

**Voorbeelden**


    exceptions | take 1 
  	| mvexpand details[0]

Hiermee splitst een uitzonderingsrecord voor een in rijen voor elk item in het veld details.



### <a name="parse-operator"></a>operator parseren

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Haalt de waarden uit een tekenreeks. Kunt eenvoudige of reguliere expressie overeenkomt.

**Syntaxis**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumenten**

* `T`: De invoer tabel.
* `kind`: 
 * `simple`(standaard): de `Match` tekenreeksen zijn gewone tekenreeksen.
 * `relaxed`: als de tekst niet als het type van een kolom parseren, wordt de kolom ingesteld op null en het parseren blijft 
 * `regex`: de `Match` tekenreeksen zijn reguliere expressies.
* `Text`: Een kolom of een andere expressie die wordt geëvalueerd als of kan worden geconverteerd naar een tekenreeks.
* *Vergelijken:* Overeenkomen met het volgende gedeelte van de tekenreeks en het verwijderen.
* *Kolom:* Het volgende gedeelte van de tekenreeks toewijzen aan deze kolom. De kolom wordt gemaakt als deze nog niet bestaat.
* *Type:* Het volgende gedeelte van de tekenreeks geparseerd als het opgegeven type, zoals int, tot op heden dubbel. 


**Geeft als resultaat**

De invoer tabel uitgebreid overeenkomstig de lijst met kolommen.

De elementen in de `with` component op zijn beurt weer worden vergeleken met de brontekst. Elk element chews uit een deel van de brontekst: 

* Een reguliere expressie of een letterlijke tekenreeks cursor de overeenkomende met de lengte van de overeenkomst.
* Een reguliere expressie kunt in een regex te analyseren, de minimalisering operator '?' zo spoedig mogelijk verplaatst naar de volgende overeenkomst.
* De naam van een kolom met een type parseert de tekst als het opgegeven type. Tenzij dergelijke = ontspannen, een mislukte parse wordt ongeldig overeenkomen met het hele patroon.
* De naam van een kolom zonder een type of met het type 'string', wordt het minimum aantal tekens naar de volgende overeenkomst gekopieerd.
* ' *' Hiermee slaat u het minimum aantal tekens naar de volgende overeenkomst. U kunt '*' aan het begin en einde van het patroon, of na een soort dan tekenreeks of tussen overeenkomen.

Alle elementen in een patroon parse moet overeenkomen; anders wordt er geen resultaten worden geproduceerd. De uitzondering op deze regel is dat als soort = ontspannen als bij het parseren van een variabele is mislukt, blijft voor de rest van het parseren.

**Voorbeelden**

*Eenvoudig:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | teller | aanwezig | Jaar
---|---|---|---
1 | 2 | SOCKS | 63

*Versoepeld:*

Wanneer de invoer een juiste overeenkomst voor elke kolom getypte bevat, produceert een beperkte parse dezelfde resultaten als een eenvoudige parseren. Maar als een van de getypte kolommen niet correct parseren, een beperkte parse doorgaat met het verwerken van de rest van het patroon, dat een eenvoudige parse stopt en niet een van de resultaten genereren.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | aanwezig | Jaar
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

resource | segment | vergrendelen | Release | vorige
---|---|---|---|---
Scheduler | 16 | 17-02/2016 08:41:00 | 17-02/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>Project-operator

    T | project cost=price*quantity, price

Selecteer de kolommen die u wilt opnemen, te hernoemen of te verwijderen en nieuwe berekende kolommen invoegen. De volgorde van de kolommen in de resultaatset is door de volgorde van de argumenten opgegeven. Alleen de kolommen die zijn opgegeven met de argumenten zijn opgenomen in het resultaat: alle andere in de invoer worden neergezet.  (Zie ook `extend`.)


**Syntaxis**

    T | project ColumnName [= Expression] [, ...]

**Argumenten**

* *T:* De invoer tabel.
* *ColumnName:* De naam van een kolom in de uitvoer wordt weergegeven. Als er geen *expressie*, een kolom met de naam die moet worden weergegeven in de invoer. [Namen](#names) zijn hoofdlettergevoelig en mag alfabetische, numerieke of '_' tekens. Gebruik `['...']` of `["..."]` citeren trefwoorden of namen met andere tekens.
* *Expressie:* Optionele scalaire expressie verwijzen naar de kolommen invoer. 

    Is het mogelijk om terug te keren een nieuwe berekende kolom met dezelfde naam als een bestaande kolom in de invoer.

**Geeft als resultaat**

Een tabel met de kolommen met de naam als argumenten en zoveel rijen als de tabel invoer.

**Voorbeeld**

In het volgende voorbeeld ziet u een aantal verschillende manipulaties die kunnen worden uitgevoerd met behulp van de `project` operator. De invoertabel `T` heeft drie kolommen van het type `int`: `A`, `B`, en `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Project opslag operator

    T | project-away column1, column2, ...

Opgegeven kolommen uitsluiten. Het resultaat bevat alle kolommen behalve die u een naam invoeren.

### <a name="range-operator"></a>bereikoperator

    range LastWeek from ago(7d) to now() step 1d

Genereert een tabel met één kolom met waarden. U ziet dat er geen een pijpleiding worden ingevoerd. 

|LastWeek|
|---|
|2015-12-05-09:10:04.627|
|2015-12-06-09:10:04.627|
|...|
|2015-12-12-09:10:04.627|



**Syntaxis**

    range ColumnName from Start to Stop step Step

**Argumenten**

* *ColumnName:* De naam van de kolom één in de uitvoertabel geplaatst.
* *Start:* De kleinste waarde in de uitvoer.
* *Stop:* De hoogste waarde in de uitvoer (of een gebonden op de hoogste waarde als *stap* stap boven deze waarde) wordt gegenereerd.
* *Stap:* Het verschil tussen twee opeenvolgende waarden. 

De argumenten moeten numerieke, datum-of timespan. Ze kunnen niet verwijzen naar kolommen van een tabel. (Als u wilt voor het berekenen van het bereik dat is gebaseerd op een tabel invoeren, gebruiken de [bereik- *functie*](#range), misschien met de [operator mvexpand](#mvexpand-operator).) 

**Geeft als resultaat**

Een tabel met één kolom *kolomnaam*, waarvan de waarden *gestart*, *Start worden*genoemd + *stap*... tot en met *stoppen*.

**Voorbeeld**  

```AIQL
range Steps from 1 to 8 step 3
```

Een tabel met één kolom genoemd `Steps` waarvan het type `long` en waarvan de waarden zijn `1`, `4`, en `7`.

**Voorbeeld**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Een tabel van middernacht in de afgelopen zeven dagen. Elke keer dat minder de functie opslaglocatie (floor) aan het begin van de dag.

**Voorbeeld**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Ziet u hoe de `range` operator kan worden gebruikt voor het maken van een kleine, ad hoc, dimensietabel die vervolgens wordt gebruikt om te introduceren nullen wanneer de brongegevens geen waarden heeft.

### <a name="reduce-operator"></a>operator verminderen

    exceptions | reduce by outerMessage

Wil soortgelijke records groeperen. Voor elke groep, de operator voert de `Pattern` het denkt dat het best beschrijft die groep, en de `Count` van records in de groep.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntaxis**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumenten**

* *ColumnName:* De kolom te onderzoeken. Het moet van het tekenreekstype.
* *Drempel:* Een waarde in het bereik {0..1}. De standaardwaarde is 0,001. Voor grote ingangen moet drempel klein. 

**Geeft als resultaat**

Twee kolommen, `Pattern` en `Count`. Patroon is in veel gevallen een volledige waarde van de kolom. In sommige gevallen kan deze algemene termen herkennen en vervangen door de variabele delen met ' *'.

Bijvoorbeeld: het resultaat van `reduce by city` kan bevatten: 

|Patroon | Aantal |
|---|---|
| SAN * | 5182 |
| Saint * | 2846 |
| Moskou | 3726 |
| \*-op-\* | 2730 |
| Parijs | 27163 |


### <a name="render-directive"></a>Richtlijn renderen

    T | render [ table | timechart  | barchart | piechart ]

Render zorgt ervoor dat de presentatie laag het weergeven van de tabel. Het laatste element van de pijp dient te zijn. Het is een alternatief voor het gebruik van de besturingselementen op het scherm, zodat u kunt een query opslaan met de methode voor een bepaalde presentatie.

### <a name="restrict-clause"></a>component beperken 

Hiermee geeft u de tabelnamen beschikbaar aan exploitanten die volgen. Bijvoorbeeld:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>operator sorteren 

    T | sort by country asc, price desc

De rijen van de tabel invoer in volgorde sorteren door een of meer kolommen.

**Alias**`order`

**Syntaxis**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumenten**

* *T:* De tabel invoer te sorteren.
* *Kolom:* Kolom van *T* waarop wordt gesorteerd. Het type van de waarden moeten numeriek, datum, tijd of tekenreeks.
* `asc`Sorteren op in oplopende volgorde, van laag naar hoog. De standaardwaarde is `desc`, aflopend hoog naar laag.

**Voorbeeld**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Alle rijen in de tabel sporen met een specifieke `ActivityId`, gesorteerd op hun tijdstempel.

### <a name="summarize-operator"></a>operator samenvatten

Een tabel die de inhoud van de tabel invoer levert.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Een tabel waarin het aantal, gemiddelde van aanvraag en de set steden in elk land. Er is een rij in de uitvoer voor elk afzonderlijk land. De kolommen tonen de telling, gemiddelde duur, steden en landen. Alle andere kolommen invoer, worden genegeerd.


    T | summarize count() by price_range=bin(price, 10.0)

Een tabel waarin wordt aangegeven hoeveel artikelen moeten prijzen in elk interval [0,10.0], [10.0,20.0], enzovoort. In dit voorbeeld is een kolom voor de telling en één voor de prijsklasse. Alle andere kolommen invoer, worden genegeerd.


**Syntaxis**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumenten**

* *Kolom:* Optionele naam voor een resultaatkolom. De standaardwaarde is een naam afgeleid van de expressie. [Namen](#names) zijn hoofdlettergevoelig en mag alfabetische, numerieke of '_' tekens. Gebruik `['...']` of `["..."]` citeren trefwoorden of namen met andere tekens.
* *Aggregatie:* Een aanroep van een aggregatiefunctie zoals `count()` of `avg()`, met de kolomnamen als argumenten. Zie [aggregaties](#aggregations).
* *GroupExpression:* Een expressie in de kolommen, waarmee een set van afzonderlijke waarden. Meestal is het een naam van een kolom die al een beperkte set waarden, of `bin()` met een kolom met numerieke of tijd als argument. 

Als u een expressie numerieke of tijd zonder `bin()`, Analytics past u deze met een interval van `1h` voor tijden, of `1.0` voor getallen.

Als u niet een *GroupExpression,* wordt de hele tabel samengevat in een rij één output.



**Geeft als resultaat**

De invoer rijen worden gerangschikt in groepen met dezelfde waarden van de `by` expressies. De functies van de opgegeven berekening worden vervolgens berekend over elke groep een rij voor elke groep produceren. Het resultaat bevat de `by` kolommen en ook ten minste één kolom Totaal berekend. (Sommige functies van aggregatie geeft meerdere kolommen).

Het resultaat heeft hetzelfde aantal rijen zijn er verschillende combinaties van `by` waarden. Als u samenvatten op het bereiken van numerieke waarden wilt, gebruikt u `bin()` te bereiken verminderen naar de afzonderlijke waarden.

**Opmerking**

Hoewel u kunt willekeurige expressies de aggregatie-en groupeerexpressies, is het efficiënter gebruiken eenvoudige kolomnamen of toepassing `bin()` een numerieke kolom.



### <a name="take-operator"></a>operator worden

Alias van [limiet](#limit-operator)


### <a name="top-operator"></a>Top-operatoren

    T | top 5 by Name desc nulls first

Deze eigenschap retourneert de eerste *N* records gesorteerd op de opgegeven kolommen.


**Syntaxis**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumenten**

* *Aantal_rijen:* Het aantal rijen van *T* om terug te keren.
* *Sort_expression:* Een expressie waarmee de rijen sorteren. Het is doorgaans alleen de kolomnaam van een. U kunt meer dan één sort_expression opgeven.
* `asc`of `desc` (de standaardinstelling) wordt mogelijk weergegeven om te bepalen of selectie daadwerkelijk uit de "bottom" of "top" van het bereik.
* `nulls first`of `nulls last` besturingselementen waarin null-waarden worden weergegeven. `First`is de standaardwaarde voor `asc`, `last` is de standaardwaarde voor `desc`.


**Tips**

`top 5 by name`Oppervlakkig gezien lijken gelijk is aan `sort by name | take 5`. Echter, sneller en altijd geeft resultaten gesorteerd dat `take` maakt geen garantie.

### <a name="top-nested-operator"></a>operator boven genest

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

De resultaten van hiërarchische, waarbij elk niveau een drilldownbewerking uit het vorige niveau is. Het is nuttig voor het beantwoorden van vragen die klinken als "wat de top 5 aanvragen zijn voor elk van hen, wat de buckets 3 topprestaties zijn en voor elk van hen zijn de top 3 landen de verzoeken afkomstig van?"

**Syntaxis**

   T | N van de kolom boven genest door SAMENVOEGING [,...]

**Argumenten**

* N:int - aantal rijen te retourneren of doorsturen naar het volgende niveau. Het totale aantal rijen in een query met drie niveaus, waarbij N staat voor 5, 3 en 3, worden 45.
* KOLOM - een kolom groeperen op voor aggregatie. 
* AGGREGATIE - een [Aggregatiefunctie](#aggregations) toe te passen op elke groep rijen. De resultaten van deze aggregaties bepaalt de bovenste groepen worden weergegeven.


### <a name="union-operator"></a>UNION-operator

     Table1 | union Table2, Table3

Heeft twee of meer tabellen en van al deze rijen als resultaat gegeven. 

**Syntaxis**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumenten**

* *Tabel1*, *tabel2* ...
 *  De naam van een tabel, zoals `requests`, of een tabel die is gedefinieerd in een die wordt [component laten](#let-clause); of
 *  Een query-expressie, zoals`(requests | where success=="True")`
 *  Een set tabellen die zijn opgegeven met een jokerteken. Bijvoorbeeld, `e*` zou de Unie van de tabellen die zijn gedefinieerd in de vorige laat componenten waarvan de naam begon met 'e', samen met de uitzonderingentabel formulier.
* `kind`: 
 * `inner`-Het resultaat is de subset van kolommen die gemeenschappelijk voor alle tabellen invoeren zijn.
 * `outer`-Het resultaat is de kolommen die zich voordoen in een van de ingangen. Cellen die niet zijn gedefinieerd door een rij invoer worden ingesteld op `null`.
* `withsource=`*ColumnName:* Als opgegeven, opgenomen in de uitvoer een kolom *kolomnaam* waarvan de waarde geeft aan welke tabel elke rij heeft bijgedragen.

**Geeft als resultaat**

Een tabel met net zoveel rijen als er in de invoertabellen, en zo veel kolommen als er unieke kolomnamen in de ingangen.

**Voorbeeld**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
De vereniging van alle tabellen waarvan de namen beginnen met 'tt'.


**Voorbeeld**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Het aantal afzonderlijke gebruikers die zijn geproduceerd op een `exceptions` gebeurtenis of een `traces` gebeurtenis op de laatste dag. In het resultaat, geeft de kolom 'SourceTable' aan 'Query' of 'Command'.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Deze versie efficiënter geeft hetzelfde resultaat. Deze filters elke tabel voor het maken van de Unie.

### <a name="where-operator"></a>waar operator

     requests | where resultCode==200

Filters in een tabel in de subset van rijen die voldoen aan een predikaat.

**Alias**`filter`

**Syntaxis**

    T | where Predicate

**Argumenten**

* *T:* Invoer in tabelvorm waarvan de records worden gefilterd.
* *Predikaat:* A `boolean` [expressie](#boolean) in de kolommen van *T*. Voor elke rij in de *T*geëvalueerd.

**Geeft als resultaat**

Rijen in *T* waarvoor *predikaat* is `true`.

**Tips**

De snelste om prestaties te behalen:

* **Gebruik eenvoudige vergelijkingen** tussen kolomnamen en -constanten zijn. ('Constante' constante op de tabel - betekent dus `now()` en `ago()` in orde zijn en zodanig zijn scalaire waarden toegewezen met behulp van een [ `let` component](#let-clause).)

    Bijvoorbeeld `where Timestamp >= ago(1d)` naar `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest eerste voorwaarden**: als u meerdere componenten met conjoined `and`, plaatst u eerst de clausules die betrekking hebben op slechts één kolom. Dus `Timestamp > ago(1d) and OpId == EventId` is beter dan andersom.


**Voorbeeld**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Records die niet ouder zijn dan 1 uur en afkomstig zijn van de bron "Kuskus" genoemd en twee kolommen van dezelfde waarde hebben. 

U ziet dat we nemen die de vergelijking tussen de twee kolommen, er kan geen gebruikmaken van de index en dwingt een scan.


### <a name="where-in-operator"></a>operator waar in

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntaxis**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumenten**

* `col`: Een kolom in de tabel.
* `expr1`...: Een lijst met scalaire expressies.

Gebruik `in` wordt gebruikt om alleen rijen opnemen waarvoor `col` gelijk is aan een van de expressies `expr1...`.

Gebruik `!in` alleen rijen opnemen waarvoor `col` is niet gelijk aan een van de expressies `expr1...`.  


## <a name="aggregations"></a>Samenvoegingen

Samenvoegingen zijn functies die worden gebruikt om waarden in groepen die zijn gemaakt in de [Samenvatting van de bewerking](#summarize-operator)te combineren. In deze query, bijvoorbeeld, is dcount() een aggregatiefunctie:

    requests | summarize dcount(name) by success

### <a name="any"></a>alle 

    any(Expression)

Willekeurig Hiermee wordt één rij van de groep en wordt de waarde van de opgegeven expressie.

Dit is bijvoorbeeld handig wanneer een kolom een groot aantal vergelijkbare waarden (bijvoorbeeld een "fout" kolom bevat) en u die kolom eenmaal per een unieke waarde van de sleutel samengestelde groep monster wilt nemen. 

**Voorbeeld**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Zoekt u een rij in de groep die *ExprToMaximize*wordt geminimaliseerd/maximaliseert en retourneert de waarde van *ExprToReturn* (of `*` voor de gehele rij retourneren).

**Tip**: de doorgegeven via kolommen worden automatisch gewijzigd. Inspecteren om te controleren of u de juiste namen gebruikt, de resultaten met behulp van `take 5` voordat u de resultaten in een andere operator pipe.

**Voorbeelden**

Voor elke naam aanvraag weergeven wanneer de langste aanvraag is opgetreden:

    requests | summarize argmax(duration, timestamp) by name

De details van de langste aanvraag, niet alleen de tijdstempel weergeven:

    requests | summarize argmax(duration, *) by name


Zoeken naar de laagste waarde van elke metrisch, samen met de tijdstempel en andere gegevens:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Gem.

    avg(Expression)

Berekent het gemiddelde van de *expressie* in de groep.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Deze eigenschap retourneert het minimale schema dat alle waarden van *DynamicExpression*admits. 

Het type van de kolom parameter moet `dynamic` -een matrix of een eigenschap zak. 

**Voorbeeld**

    exceptions | summarize buildschema(details)

Resultaat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Merk op dat `indexer` om te markeren waar moet u een numerieke index wordt gebruikt. Voor dit schema enkele geldige paden zou worden (mits dat deze indexen voorbeeld zijn bereik):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Voorbeeld**

Stel dat de kolom invoer heeft drie dynamische waarden:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Het resulterende schema zou zijn:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Het schema vertelt ons dat:

* Het hoofdobject is een container met vier eigenschappen x, y, z en t.
* De eigenschap met de naam 'x', die kan worden van het type "int" of van het type 'string'.
* De eigenschap "y", dat kan zijn van een van beide van het type "double" genoemd, of een andere container met een eigenschap genoemd "w" van het type 'string'.
* De ``indexer`` trefwoord geeft aan dat "z" en "t" matrices zijn.
* Elk item in de array 'z' is een geheel getal of een tekenreeks.
* "t" is een matrix van tekenreeksen.
* Elke eigenschap is impliciet optioneel en is de array leeg.

##### <a name="schema-model"></a>Schema-model

De syntaxis van het resulterende schema is:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Ze zijn gelijkwaardig aan een subset van de schrijfmachine typeannotaties, gecodeerd als een dynamische waarde. Schrijfmachine, zou het voorbeeldschema zijn:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>aantal

    count([ Predicate ])

Retourneert een telling van de rijen waarvoor *predikaat* resulteert in `true`. Als er geen *predikaat* is opgegeven, geeft als resultaat het totale aantal records in de groep. 

**Perf tip**: Gebruik `summarize count(filter)` in plaats van`where filter | summarize count()`

> [AZURE.NOTE] Vermijd het gebruik van count() het aantal aanvragen, uitzonderingen of andere gebeurtenissen die hebben plaatsgevonden. Wanneer [de bemonstering](app-insights-sampling.md) uitgevoerd wordt, is het aantal gegevenspunten behouden in inzichten van toepassing lager dan het nummer van de oorspronkelijke gebeurtenissen. Gebruik in plaats daarvan `summarize sum(itemCount)...`. De eigenschap itemCount geeft het nummer van de oorspronkelijke gebeurtenissen die worden vertegenwoordigd door elk gegevenspunt ingehouden.

### <a name="countif"></a>aantal

    countif(Predicate)

Retourneert een telling van de rijen waarvoor *predikaat* resulteert in `true`.

**Perf tip**: Gebruik `summarize countif(filter)` in plaats van`where filter | summarize count()`

> [AZURE.NOTE] Vermijd het gebruik van countif() om het aantal aanvragen, uitzonderingen of andere gebeurtenissen die hebben plaatsgevonden. Wanneer [de bemonstering](app-insights-sampling.md) uitgevoerd wordt, is het aantal gegevenspunten lager dan het aantal werkelijke gebeurtenissen. Gebruik in plaats daarvan `summarize sum(itemCount)...`. De eigenschap itemCount geeft het nummer van de oorspronkelijke gebeurtenissen die worden vertegenwoordigd door elk gegevenspunt ingehouden.

### <a name="dcount"></a>DCount

    dcount( Expression [ ,  Accuracy ])

Geeft als resultaat een schatting van het aantal afzonderlijke waarden van *Expr* in de groep. (U kunt de afzonderlijke waarden gebruiken [`makeset`](#makeset).)

*Nauwkeurigheid*, indien opgegeven, bepaalt de verhouding tussen snelheid en nauwkeurigheid.

 * `0`= de minste nauwkeurige en snelste berekening.
 * `1`Standaard en saldi van de juistheid en berekening van tijd; over 0,8% fout.
 * `2`meest berekening nauwkeurige en langzaamste; over 0,4% fout.

**Voorbeeld**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Geeft als resultaat een schatting van het aantal afzonderlijke waarden van *Expr* van rijen in de groep waarvoor *predikaat* ingesteld op true is. (U kunt de afzonderlijke waarden gebruiken [`makeset`](#makeset).)

*Nauwkeurigheid*, indien opgegeven, bepaalt de verhouding tussen snelheid en nauwkeurigheid.

 * `0`= de minste nauwkeurige en snelste berekening.
 * `1`Standaard en saldi van de juistheid en berekening van tijd; over 0,8% fout.
 * `2`meest berekening nauwkeurige en langzaamste; over 0,4% fout.

**Voorbeeld**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Deze eigenschap retourneert een `dynamic` (JSON) matrix met alle waarden van *Expr* in de groep. 

* *MaxListSize* is een optioneel getal limiet op het maximum aantal elementen als resultaat gegeven (de standaardwaarde is *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Deze eigenschap retourneert een `dynamic` (JSON) matrix van de set van afzonderlijke waarden die *Expr* in de groep neemt. (Tip: gebruiken om alleen de afzonderlijke waarden tellen, [`dcount`](#dcount).)
  
*  *MaxSetSize* is een optioneel getal limiet op het maximum aantal elementen als resultaat gegeven (de standaardwaarde is *128*).

**Voorbeeld**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Zie ook de [ `mvexpand` operator](#mvexpand-operator) voor de omgekeerde functie.


### <a name="max-min"></a>Max, min

    max(Expr)

Berekent de maximale waarde van *Expr*.
    
    min(Expr)

Het minimum van *Expr*berekend.

**Tip**: dit geeft u min of max op eigen - bijvoorbeeld, de hoogste of laagste prijs. Maar als u wilt dat andere kolommen in de rij - bijvoorbeeld de naam van de leverancier met de laagste prijs - [argmin of argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>percentiel, percentielen, percentilew, percentilesw

    percentile(Expression, Percentile)

Geeft als resultaat een schatting voor de *expressie* van het opgegeven percentiel in de groep. De nauwkeurigheid is afhankelijk van de dichtheid van de bevolking in de regio van het percentiel.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Net als `percentile()`, maar een aantal waarden percentiel (die sneller is dan berekening afzonderlijk van elke percentiel) berekend.

    percentilew(Expression, WeightExpression, Percentile)

Gewogen percentiel. Gebruik deze optie voor vooraf samengevoegde gegevens.  `WeightExpression`is een geheel getal dat aangeeft hoeveel rijen van de oorspronkelijke worden vertegenwoordigd door elke toegevoegde rij.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Net als `percentilew()`, maar een aantal waarden percentiel wordt berekend.

**Voorbeelden**


De waarde van `duration` die groter is dan 95% van de steekproef is ingesteld en kleiner zijn dan 5% van de voorbeeldset berekend voor elke aanvraag:

    request 
  	| summarize percentile(duration, 95)
      by name

Weglaten ' door... "om te berekenen voor de volledige tabel.

Tegelijkertijd verschillende percentielen voor namen van de verschillende aanvraag berekenen:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

De resultaten weergeven die voor de aanvraag /Events/Index, 5% van de aanvragen worden beantwoord in minder dan 2.44s, de helft van hen in 3.52s, en 5% langzamer dan 6.85s zijn.

Meerdere statistieken te berekenen:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Gewogen percentielen

Gebruik de functies gewogen percentiel in gevallen waarin de gegevens vooraf samengevoegde zijn. 

Stel bijvoorbeeld dat uw app duizenden bewerkingen per seconde wordt uitgevoerd en u wilt weten van de latentie. De eenvoudige oplossing zou zijn voor het genereren van een toepassing inzichten aanvraag of aangepaste gebeurtenis voor elke bewerking. Dit maakt veel verkeer, hoewel adaptieve bemonstering van kracht worden te verkleinen. Maar dat u besluit om een nog betere oplossing te implementeren: u schrijft wat code in uw app voor het onderbrengen van de gegevens voordat u deze verzendt naar de inzichten van toepassing. Samenvatting van de geaggregeerde ontvangt regelmatig de gegevenssnelheid misschien beperken tot een aantal punten per minuut.

De code wordt een stroom van de metingen van de latentie in milliseconden. Bijvoorbeeld:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Het telt de waarden in de volgende opslaglocaties:`{ 10, 20, 30, 40, 50, 100 }`

Het maakt regelmatig een reeks aanroepen van TrackEvent, één voor elke bucket, met aangepaste afmetingen in elk gesprek: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

In Analytics ziet u een dergelijke groep van gebeurtenissen als volgt:

`opCount` | `latency`| betekenis
---|---|---
8 | 10 | 8 de werking in de opslaglocatie 10ms =
6 | 20 | 6 bewerkingen in de opslaglocatie 20 MS =
3 | 30 | = 3 bewerkingen in de opslaglocatie 30ms
1 | 40 | = 1 bewerkingen in de opslaglocatie voor 40ms

Als u een nauwkeurig overzicht van de oorspronkelijke verdeling van de gebeurtenis vertragingstijden, gebruiken we `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

De resultaten zijn hetzelfde als we had gebruikt zonder opmaak `percentiles` op de oorspronkelijke set van metingen.

> [AZURE.NOTE] Gewogen percentielen zijn niet van toepassing op de [gegevens wordt bemonsterd](app-insights-sampling.md), waarbij elke bemonsterde rij vertegenwoordigt een aselecte steekproef van de oorspronkelijke rijen in plaats van een opslaglocatie. De gewone percentiel functies zijn geschikt voor de bemonsterde gegevens.

#### <a name="estimation-error-in-percentiles"></a>Fout in percentielen schatting

De samengestelde percentielen biedt een geschatte waarde met [T-Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Een paar belangrijke punten: 

* De grenzen voor de fout van de raming is afhankelijk van de waarde van de aangevraagde percentiel. De nauwkeurigheid is aan het einde van de [0..100], schalen, percentielen 0 en 100 zijn de exacte minimum- en maximumwaarden van de verdeling. De nauwkeurigheid-verlaagt geleidelijk naar het midden van de schaal. Het ergste op de mediaan is en is beperkt tot 1%. 
* Fout grenzen worden waargenomen op de rang, niet op de waarde. Stel dat percentiel (X 50) van de Xm-waarde geretourneerd. De schatting zorgt ervoor dat ten minste 49% en maximaal 51% van de X-waarden lager dan Xm zijn. Er is geen theoretische limiet van het verschil tussen Xm en de gemiddelde waarde van X.

### <a name="stdev"></a>STDEV

     stdev(Expr)

Geeft als resultaat de standaarddeviatie van *Expr* voor de groep.

### <a name="variance"></a>variantie

    variance(Expr)

Geeft als resultaat de variantie van *Expr* voor de groep.

### <a name="sum"></a>som

    sum(Expr)

Berekent de som van *Expr* voor de groep.                      


## <a name="scalars"></a>Hoeken

[webcasts](#casts) | [vergelijken](#scalar-comparisons)
<br/>
[GETTYPE](#gettype) | [hash](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

De ondersteunde typen zijn:

| Type      | Als u meer namen   | Gelijkwaardige .NET-type |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Webcasts

U kunt converteren van het ene type naar het andere. In het algemeen als de conversie zinvol, werkt het:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Controleren of een tekenreeks kan worden omgezet naar een specifiek type:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Scalaire vergelijkingen.

||
---|---
`<` |Minder
`<=`|Kleiner of gelijk aan
`>` |Groter
`>=`|Groter of gelijk aan
`<>`|Is niet gelijk aan
`!=`|Is niet gelijk aan 
`in`| Rechter operand is een matrix (dynamische) en de linker operand is gelijk aan een van de elementen.
`!in`| Rechter operand is een matrix (dynamische) en de linker operand is niet gelijk aan een van de elementen.




### <a name="gettype"></a>GETTYPE

**Geeft als resultaat**

Een tekenreeks die het onderliggende opslagtype van één argument vertegenwoordigt. Dit is vooral handig wanneer u de waarden van het type `dynamic`: in dit geval `gettype()` , ontdekt u hoe een waarde wordt gecodeerd.

**Voorbeelden**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>hash

**Syntaxis**

    hash(source [, mod])

**Argumenten**

* *bron*: de bron scalaire door de hash wordt berekend op.
* *Mod*: de modulo-waarde moet worden toegepast op het hash-resultaat.

**Geeft als resultaat**

De xxhash (lang) waarde van de opgegeven scalaire modulo gegeven mod waarde (indien opgegeven).

**Voorbeelden**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

De `iff()` functie evalueert het eerste argument (predicaat), en geeft als resultaat de waarde van de tweede of derde argumenten afhankelijk van of het predicaat `true` of `false`. De tweede en derde argumenten moeten van hetzelfde type.

**Syntaxis**

    iff(predicate, ifTrue, ifFalse)


**Argumenten**

* *predicaat:* Een expressie die resulteert in een `boolean` waarde.
* *ifTrue:* Een expressie die wordt geëvalueerd en de waarde geretourneerd van de functie als *predicaat* als `true`.
* *ifFalse:* Een expressie die wordt geëvalueerd en de waarde geretourneerd van de functie als *predicaat* als `false`.

**Geeft als resultaat**

Deze functie retourneert de waarde van *ifTrue* als *predicaat* resulteert in `true`, of de waarde van *ifFalse* anders.

**Voorbeeld**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, isnotnull, notnull

    isnull(parsejson("")) == true

Een enkel argument en wordt aangegeven of de waarde is leeg.

**Syntaxis**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Geeft als resultaat**

Waar of ONWAAR, afhankelijk van of wordt de waarde null of not null.


|x|IsNull(x)
|---|---
| "" | False
|"x" | False
|parsejson("")|True
|parsejson("[]")|False
|parsejson("{}")|False

**Voorbeeld**

    T | where isnotnull(PossiblyNull) | count

Zoals u ziet, er zijn andere manieren om dit effect te bereiken:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Een query of expressie geëvalueerd en wordt het resultaat als een enkele waarde. Deze functie is handig voor gefaseerde berekeningen; bijvoorbeeld, berekening van het totale aantal gebeurtenissen en die vervolgens als basis te gebruiken.

**Syntaxis**

    toscalar(query)
    toscalar(scalar)

**Geeft als resultaat**

Het argument geëvalueerd. Als het argument een tabel, wordt de eerste kolom van de eerste rij. (Het wordt aangeraden rangschikken dat het argument slechts één kolom en rij heeft.)

**Voorbeeld**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Letterlijke Booleaanse

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Boole-operators

    and 
    or 

    

## <a name="numbers"></a>Nummers

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numerieke letterlijke waarden

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Rekenkundige operatoren

|| |
|---|-------------|
| + | Toevoegen         |
| - | Aftrekken    |
| * | Vermenigvuldigen    |
| / | Verdelen      |
| % | Modulo      |
||
|`<` |Minder
|`<=`|Kleiner of gelijk aan
|`>` |Groter
|`>=`|Groter of gelijk aan
|`<>`|Is niet gelijk aan
|`!=`|Is niet gelijk aan 


### <a name="abs"></a>ABS

**Syntaxis**

    abs(x)

**Argumenten**

* x - een geheel getal, reëel of timespan

**Geeft als resultaat**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>opslaglocatie, verdieping

Waarden naar een integer Rondt veelvoud van de grootte van een bepaalde opslaglocatie. Veel gebruikt in de [`summarize by`](#summarize-operator) query. Als er een verspreide set van waarden, worden deze gegroepeerd in een kleiner aantal specifieke waarden.

Alias `floor`.

**Syntaxis**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumenten**

* *waarde:* Een getal, datum of timespan. 
* *roundTo:* De "bin formaat". Een getal, datum of tijdsduur die *waarde*wordt gedeeld. 

**Geeft als resultaat**

Het dichtstbijzijnde veelvoud van *roundTo* onder *waarde*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Voorbeelden**

Expressie | Resultaat
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


De volgende expressie wordt een histogram van duur, met een Bucketgrootte van 1 seconde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>EXP

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>Floor

Een alias voor [`bin()`](#bin).

### <a name="gamma"></a>gamma

De [gamma-functie](https://en.wikipedia.org/wiki/Gamma_function)

**Syntaxis**

    gamma(x)

**Argumenten**

* *x:* Een reëel getal

Voor de positieve gehele getallen, `gamma(x) == (x-1)!` , `gamma(5) == 4 * 3 * 2 * 1`.

Zie ook [loggamma](#loggamma).


### <a name="log"></a>logboek

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`moet een reëel getal > 0. Anders wordt null geretourneerd.

### <a name="loggamma"></a>loggamma


De natuurlijke logaritme van de absolute waarde van de [gamma-functie](#gamma).

**Syntaxis**

    loggamma(x)

**Argumenten**

* *x:* Een reëel getal


### <a name="rand"></a>ASELECT

Een random number generator.

* `rand()`-een reëel getal tussen 0.0 en 1.0
* `rand(n)`-een geheel getal tussen 0 en n-1




### <a name="sqrt"></a>WORTEL

De functie voor worteltrekken.  

**Syntaxis**

    sqrt(x)

**Argumenten**

* *x:* Een reëel getal > = 0.

**Geeft als resultaat**

* Een positief getal dat`sqrt(x) * sqrt(x) == x`
* `null`Als het argument negatief is of niet kan worden geconverteerd naar een `real` waarde. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>ToLong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>ToDouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum en tijd


[geleden](#ago) | [dayofmonth](#dayofmonth) | [dag van de week](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [nu](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Letterlijke datum en tijd

|||
---|---
**datum/tijd**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Tijden zijn altijd in UTC. Als de datum geeft vandaag nog een keer.
`now()`|De huidige tijd.
`now(`-*TimeSpan*`)`|`now()-`*TimeSpan*
`ago(`*TimeSpan*`)`|`now()-`*TimeSpan*
**TimeSpan**|
`2d`|2 dagen
`1.5h`|1,5 uur 
`30m`|30 minuten
`10s`|10 seconden
`0.1s`|0,1 seconde
`100ms`| van 100 milliseconden
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 dagen
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Expressies voor datum en tijd

Expressie |Resultaat
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Minder
`<=`|Kleiner of gelijk aan
`>` |Groter
`>=`|Groter of gelijk aan
`<>`|Is niet gelijk aan
`!=`|Is niet gelijk aan 




### <a name="ago"></a>geleden

Hiermee verwijdert u het opgegeven interval van de huidige tijd van UTC klok. Net als `now()`, deze functie kan meerdere keren worden gebruikt in een instructie en de UTC-kloktijd waarnaar wordt verwezen is hetzelfde voor alle exemplaren.

**Syntaxis**

    ago(a_timespan)

**Argumenten**

* *a_timespan*: Interval moet worden afgetrokken van de huidige tijd van UTC klok (`now()`).

**Geeft als resultaat**

    now() - a_timespan

**Voorbeeld**

Alle rijen met een tijdstempel in het afgelopen uur:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Een deel van een datum als een geheel worden uitgepakt.

**Syntaxis**

    datepart(part, datetime)

**Argumenten**

* `part:String`-{'Jaar', 'Maand', 'Dag', 'Uur', "Minuten", "Tweede", "MS", "Microseconde", "Nanoseconden"}
* `datetime`

**Geeft als resultaat**

Een Long die het opgegeven onderdeel voorstelt.


### <a name="dayofmonth"></a>DayOfMonth

    dayofmonth(datetime("2016-05-15")) == 15 

De ordinale waarde van de dag van de maand.

**Syntaxis**

    dayofmonth(a_date)

**Argumenten**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>dag van de week

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Het gehele aantal dagen sinds de voorafgaande zondag, als een `timespan`.

**Syntaxis**

    dayofweek(a_date)

**Argumenten**

* `a_date`: A `datetime`.

**Geeft als resultaat**

De `timespan` sinds middernacht aan het begin van de voorafgaande zondag, naar beneden afgerond op een geheel getal van dagen.

**Voorbeelden**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

De ordinale waarde van de dag van het jaar.

**Syntaxis**

    dayofyear(a_date)

**Argumenten**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday, endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Het maandnummer van de (1-12) worden opgehaald uit een datum/tijd.

**Voorbeeld**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Het jaar ophalen uit een datum/tijd.

**Voorbeeld**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>nu

    now()
    now(-2d)

De huidige UTC kloktijd wordt eventueel gecompenseerd door een bepaalde tijdsduur. Deze functie kan meerdere keren worden gebruikt in een instructie en de kloktijd waarnaar wordt verwezen is hetzelfde voor alle exemplaren.

**Syntaxis**

    now([offset])

**Argumenten**

* *offset:* A `timespan`, toegevoegd aan de huidige tijd van UTC klok. Standaard: 0.

**Geeft als resultaat**

De huidige tijd van UTC klok als een `datetime`.

    now() + offset

**Voorbeeld**

Bepaalt het interval na de gebeurtenis die wordt aangeduid met het predikaat:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday, startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Controleren of een tekenreeks een geldige datum is:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Het resultaat geheel getal geeft het weeknummer van de door het standaard ISO 8601. De eerste dag van een week is zondag en de eerste week van het jaar is de week waarin de eerste donderdag van het jaar. (De laatste dagen van een jaar kunnen bevatten enkele van de dagen van de week 1 van het volgende jaar, of de eerste dagen kunnen bevatten sommige week 52 of 53 van het voorgaande jaar).


## <a name="string"></a>Tekenreeks

[countof](#countof) | [extract](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [vervangen door](#replace) | [splitsen](#split) | [strcat](#strcat) | [strlen](#strlen) | [subtekenreeks](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Letterlijke tekenreeks

De regels zijn hetzelfde als in JavaScript.

Tekenreeksen kunnen worden ingesloten in enkele of dubbele aanhalingstekens. 

Backslash (`\`) wordt gebruikt voor de escape-tekens, zoals `\t` (tabblad), `\n` (nieuwe regel) en exemplaren van het overkoepelende aanhalingsteken.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Letterlijke tekenreeksen betekenisloze

Betekenisloze letterlijke tekenreeksen zijn tekenreeksen die Analytics zichtbaar bij het uitvoeren van de tekenreeks (bijvoorbeeld traceren). Het proces codeversleuteling vervangt alle verborgen tekens door een begin (`*`) teken.

Voeg een betekenisloze letterlijke tekenreeks vormen, `h` of 'H'. Bijvoorbeeld:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Tekenreeksvergelijkingen

Operator|Beschrijving|Hoofdlettergevoelig|Voorbeeld van de eigenschap is True
---|---|---|---
`==`|Is gelijk aan |Ja| `"aBc" == "aBc"`
`<>` `!=`|Is niet gelijk aan|Ja| `"abc" <> "ABC"`
`=~`|Is gelijk aan |Nee| `"abc" =~ "ABC"`
`!~`|Is niet gelijk aan |Nee| `"aBc" !~ "xyz"`
`has`|Rechts-rechter-zijde (RHS) is een volledige term in de linker-rechter-zijde (LHS)|Nee| `"North America" has "america"`
`!has`|RHS is niet een volledige term in LHS|Nee|`"North America" !has "amer"` 
`hasprefix`|RHS is een voorvoegsel van een term in de LHS|Nee|`"North America" hasprefix "ame"`
`!hasprefix`|RHS is niet een voorvoegsel van een term in de LHS|Nee|`"North America" !hasprefix "mer"`
`hassuffix`|RHS is een achtervoegsel van een term in de LHS|Nee|`"North America" hassuffix "rth"`
`!hassuffix`|RHS is niet een achtervoegsel van elke term in LHS|Nee|`"North America" !hassuffix "mer"`
`contains` | RHS treedt op als een subtekenreeks van LHS|Nee| `"FabriKam" contains "BRik"`
`!contains`| RHS komt niet voor in de LHS|Nee| `"Fabrikam" !contains "xyz"`
`containscs` | RHS treedt op als een subtekenreeks van LHS|Ja| `"FabriKam" contains "Kam"`
`!containscs`| RHS komt niet voor in de LHS|Ja| `"Fabrikam" !contains "Kam"`
`startswith`|RHS is een eerste reeks van LHS.|Nee|`"Fabrikam" startswith "fab"`
`!startswith`|RHS is een eerste reeks van LHS.|Nee|`"Fabrikam" !startswith "abr"`
`endswith`|RHS is LHS terminal subreeks.|Nee|`"Fabrikam" endswith "kam"`
`!endswith`|RHS is LHS terminal subreeks.|Nee|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS een overeenkomst bevat voor RHS|Ja| `"Fabrikam" matches regex "b.*k"`
`in`|Gelijk aan een van de elementen|Ja|`"abc" in ("123", "345", "abc")`
`!in`|Is niet gelijk aan een van de elementen|Ja|`"bc" !in ("123", "345", "abc")`

Gebruik `has` of `in` als u op de aanwezigheid van een hele lexicale term testen wilt - dat wil zeggen, een symbool of een alfanumerieke woord dat wordt begrensd door niet-alfanumerieke tekens of het begin of einde van veld. `has`voert sneller dan `contains`, `startswith` of `endswith`. De eerste van deze query's sneller worden uitgevoerd:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Telt het aantal exemplaren van een subtekenreeks van een tekenreeks. Gewone overeenkomen kunnen overlappen; Regex overeenkomsten niet.

**Syntaxis**

    countof(text, search [, kind])

**Argumenten**

* *tekst:* Een tekenreeks.
* *Zoeken:* De gewone string of reguliere expressie overeenkomt met in *tekst*.
* *type:* `"normal"|"regex"` Standaard `normal`. 

**Geeft als resultaat**

Het aantal keren dat de zoekreeks in de container kan worden afgestemd. Gewone overeenkomen kunnen overlappen; Regex overeenkomsten niet.

**Voorbeelden**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (niet 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>extract

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Een overeenkomst voor een [reguliere expressie](#regular-expressions) worden opgehaald uit een tekenreeks. Eventueel vervolgens converteert de uitgepakte subtekenreeks naar het aangegeven type.

**Syntaxis**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumenten**

* *regex:* Een [reguliere expressie](#regular-expressions).
* *captureGroup:* Een positieve `int` -constante die de groep opnemen om uit te pakken. 0 staat voor de gehele overeenkomst, 1 voor de waarde die overeenkomt met de eerste '(' haakje')' in de reguliere expressie, 2 of meer voor de volgende ronde haakjes.
* *tekst:* A `string` te zoeken.
* *typeLiteral:* Een optionele type letterlijke waarde (bijv. `typeof(long)`). Als u opgeeft, wordt de uitgepakte subtekenreeks geconverteerd naar dit type. 

**Geeft als resultaat**

Als *regex* een overeenkomst wordt gevonden in de *tekst*: de subtekenreeks die wordt vergeleken met het vastleggen van de aangegeven groep *captureGroup*, eventueel geconverteerd naar *typeLiteral*.

Als er geen overeenkomst, of het typeconversie is mislukt: `null`. 

**Voorbeelden**

Het voorbeeld van een tekenreeks `Trace` wordt gezocht naar een definitie voor `Duration`. De overeenkomst wordt omgezet in `real`, en vervolgens het vermenigvuldigd met een constante (`1s`) dat `Duration` is van het type `timespan`. In het volgende voorbeeld is gelijk aan 123.45 seconden:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

In het volgende voorbeeld is gelijk aan `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

    isempty("") == true

Deze eigenschap is True als het argument een lege tekenreeks of null is.
Zie ook [isnull](#isnull).


**Syntaxis**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Geeft als resultaat**

Geeft aan of het argument een lege tekenreeks of isnull.

|x|IsEmpty(x)
|---|---
| "" | True
|"x" | False
|parsejson("")|True
|parsejson("[]")|False
|parsejson("{}")|False


**Voorbeeld**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>ParseURL

Een URL in de delen wordt gesplitst.

**Syntaxis**

    parseurl(urlstring)

**Argumenten**

* *urlstring:* EEN URL.

**Geeft als resultaat**

Een object met de onderdelen als tekenreeksen.

**Voorbeeld**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>vervangen

Alle resultaten van de regex vervangen door een andere tekenreeks.

**Syntaxis**

    replace(regex, rewrite, text)

**Argumenten**

* *regex:* De [reguliere expressie](https://github.com/google/re2/wiki/Syntax) naar *tekst*wilt zoeken. Het kan vastleggen van groepen haakjes"(" ")' bevatten. 
* *schrijven:* De regex vervangen voor een overeenkomst die door *matchingRegex*. Gebruik `\0` om te verwijzen naar de gehele overeenkomst `\1` voor de eerste groep voor het vastleggen `\2` enzovoort voor groepen van de volgende opname.
* *tekst:* Een tekenreeks.

**Geeft als resultaat**

*tekst* na alle resultaten van de *regex* vervangen door evaluaties van *herschrijven*. Komt overeen met elkaar niet overlappen.

**Voorbeeld**

Deze instructie:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Heeft de volgende resultaten:

| x    | Str | vervangen|
|---|---|---|
| 1    | Het nummer is 1.000000  | Het nummer is: 1.000000|
| 2    | Het nummer is 2.000000  | Het nummer is: 2.000000|
| 3    | Het nummer is 3.000000  | Het nummer is: 3.000000|
| 4    | Het nummer is 4.000000  | Het nummer is: 4.000000|
| 5    | Het nummer is 5.000000  | Het nummer is: 5.000000|
 



### <a name="split"></a>splitsen

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Splitst een bepaalde tekenreeks op basis van een opgegeven scheidingsteken en geeft als resultaat een tekenreeksmatrix met de subtekenreeksen conatined. Eventueel, een specifieke reeks kan worden geretourneerd als bestaat.

**Syntaxis**

    split(source, delimiter [, requestedIndex])

**Argumenten**

* *bron*: de brontekenreeks die indexverificatiefase volgens het opgegeven scheidingsteken.
* *scheidingsteken*: het scheidingsteken dat wordt gebruikt om de brontekenreeks splitsen.
* *requestedIndex*: een op nul gebaseerde index voor optionele `int`. Als gegeven, bevat de geretourneerde tekenreeksmatrix de gevraagde subtekenreeks als bestaat. 

**Geeft als resultaat**

Een reeksmatrix met de subtekenreeksen die worden gescheiden door scheidingsteken voor de opgegeven brontekenreeks van de opgegeven.

**Voorbeelden**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Tussen 1 en 16 argumenten moeten tekenreeksen worden aaneengeschakeld.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

De lengte van een tekenreeks.

### <a name="substring"></a>subtekenreeks

    substring("abcdefg", 1, 2) == "bc"

Een subtekenreeks ophalen uit een bepaalde bron vanaf een bepaalde index. De lengte van de opgegeven subtekenreeks kan desgewenst worden opgegeven.

**Syntaxis**

    substring(source, startingIndex [, length])

**Argumenten**

* *bron:* De brontekenreeks die de subtekenreeks wordt gehaald.
* *startingIndex:* De op nul gebaseerde beginpositie van de opgegeven subtekenreeks.
* *lengte:* Een optionele parameter die kan worden gebruikt voor het opgeven van het gewenste aantal tekens in de subtekenreeks. 

**Geeft als resultaat**

Een subtekenreeks van de opgegeven tekenreeks. De subtekenreeks begint met tekenpositie startingIndex (vanaf nul) en gaat door tot het einde van de tekenreeks of lengte tekens als opgegeven.

**Voorbeelden**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Een tekenreeks omgezet in kleine letters.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

Een tekenreeks omgezet in hoofdletters.



### <a name="guids"></a>GUID 's

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Arrays, objecten en dynamische

[letterlijke](#dynamic-literals) | [casting](#casting-dynamic-objects) | [operatoren](#operators) | [laten componenten](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [range](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Hier is het resultaat van een query op een uitzondering inzichten van toepassing. De waarde in `details` is een matrix.

![](./media/app-insights-analytics-reference/310.png)

**Indexeren:** Index arrays en objecten, net als in JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Maar `arraylength` en andere functies Analytics (niet '.length'!)

**Casting** In sommige gevallen is het nodig te gieten van een element dat u uit een object ophaalt, omdat het type kan variëren. Bijvoorbeeld `summarize...to` moet een specifiek type:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Letterlijke waarden** Maken van een expliciete array of het object van de eigenschap zak, als het een JSON-string en cast schrijven:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Gebruiken om op te halen uit elkaar de eigenschappen van een object in afzonderlijke rijen, mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Alle paden in een complexe object zoeken:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** De minimale schema dat alle waarden van de expressie in de tabel admits zoeken:

    exceptions | summarize buildschema(details)

Resultaat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Merk op dat `indexer` om te markeren waar moet u een numerieke index wordt gebruikt. Voor dit schema enkele geldige paden zou worden (mits dat deze indexen voorbeeld zijn bereik):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Letterlijke tekens voor arrays en objecten

U kunt maken van een dynamische letterlijke `parsejson` (alias `todynamic`) met een JSON argument de tekenreeks:

* `parsejson('[43, 21, 65]')`-een matrix van getallen
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-een enkele waarde van dynamisch type met een getal
* `parsejson('"21"')`-een enkele waarde van dynamisch type met een tekenreeks

Opmerking dat de JSON in tegenstelling tot JavaScript, het gebruik van dubbele aanhalingstekens oplegt (`"`) tekenreeksen. Daarom is meestal gemakkelijker citeren JSON-gecodeerde string literals gebruik enkele aanhalingstekens (`'`).

In dit voorbeeld maakt een dynamische waarde en gebruikt vervolgens de velden:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dynamisch object-functies

|||
|---|---|
| *waarde* `in` *matrix*| True als een element van een *matrix* die *waarde* ==<br/>`where City in ('London', 'Paris', 'Rome')`
| *waarde* `!in` *matrix*| De waarde True als er geen element van de *matrix is* die *waarde* ==
|[`arraylength(`matrix`)`](#arraylength)| Null als het niet een matrix
|[`extractjson(`pad, object`)`](#extractjson)|Pad gebruikt om te navigeren naar het object.
|[`parsejson(`bron`)`](#parsejson)| Een JSON-string verandert in een dynamisch object.
|[`range(`uit, stap`)`](#range)| Een matrix met waarden
|[`mvexpand`listColumn](#mvexpand-operator) | Een rij voor elke waarde in een lijst in een bepaalde cel repliceert.
|[`summarize buildschema(`kolom`)`](#buildschema) |Het type schema afleidt uit kolominhoud
|[`summarize makelist(`kolom`)`](#makelist)| Groepen rijen worden samengevoegd en worden de waarden van de kolom in een matrix.
|[`summarize makeset(`kolom`)`](#makeset) | Groepen rijen worden samengevoegd en plaatst u de waarden van de kolom in een matrix, zonder duplicatie.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamische objecten in laat componenten


[Componenten laten](#let-clause) winkel dynamische waarden als tekenreeksen, dus deze twee componenten zijn gelijk en moet de `parsejson` (of `todynamic`) alvorens te worden gebruikt:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Het aantal elementen in een dynamische matrix.

**Syntaxis**

    arraylength(array)

**Argumenten**

* *matrix:* A `dynamic` waarde.

**Geeft als resultaat**

Het aantal elementen in de *matrix*, of `null` als *matrix* niet een matrix is.

**Voorbeelden**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Ophalen van een opgegeven element uit een JSON-tekst met behulp van een padexpressie voor een. Eventueel de uitgepakte tekenreeks converteren naar een bepaald type.


**Syntaxis**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Geeft als resultaat**

Deze functie voert een query voor JsonPath in een gegevensbron met een geldige tekenreeks JSON, eventueel die waarde te converteren naar een ander type afhankelijk van het derde argument.



**Voorbeeld**

De notatie [haak] en de puntnotatie zijn equivalent:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Tips voor betere prestaties**

* Where-componenten van toepassing zijn voordat u`extractjson()`
* Overwegen een overeenkomst reguliere expressie met [pakken](#extract) gebruiken. Dit kan zeer veel sneller worden uitgevoerd en is van kracht als de JSON wordt geproduceerd op basis van een sjabloon.
* Gebruik `parsejson()` als u meer dan één waarde in de JSON uitpakken.
* Overweeg om de JSON bij inslikken geparseerd door het declareren van het type van de kolom dynamisch.

### <a name="json-path-expressions"></a>Pad naar JSON-expressies

|||
|---|---|
|`$`|Hoofdobject|
|`@`|Huidige-object|
|`[0]`|Subscript van de matrix|
|`.`of`[0]` | Kind|

*(We niet op dit moment implementeren jokertekens, recursie, union of segmenten.)*




### <a name="parsejson"></a>parsejson

Interpreteert een `string` als een [JSON-waarde](http://json.org/)) en retourneert de waarde als `dynamic`. Is het gebruik van superieure `extractjson()` wanneer u moet meer dan één element van een samengestelde JSON-object ophalen.

**Syntaxis**

    parsejson(json)

**Argumenten**

* *json:* Een JSON-document.

**Geeft als resultaat**

Een object van het type `dynamic` opgegeven met *json*.

**Voorbeeld**

In het volgende voorbeeld wordt bij het `context_custom_metrics` is een `string` dat zo uitziet: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

en vervolgens het volgende fragment, de waarde van haalt de `duration` sleuf in het object, en dat deze twee sleuven haalt `duration.value` en  `duration.min` (`118.0` en `110.0`, respectievelijk).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>bereik

De `range()` functie (niet te verwarren met de `range` operator) genereert een dynamische matrix met een reeks waarden tussenafstand.

**Syntaxis**

    range(start, stop, step)

**Argumenten**

* *start:* De waarde van het eerste element in de resulterende matrix. 
* *stop:* De waarde van het laatste element in de resulterende matrix of de laagste waarde die groter is dan het laatste element in de resulterende matrix en binnen een geheel veelvoud van *stap* van *start*.
* *stap:* Het verschil tussen twee opeenvolgende elementen van de matrix.

**Voorbeelden**

In het volgende voorbeeld wordt `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

In het volgende voorbeeld geeft als resultaat een matrix met alle dagen in het jaar 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Een tekenreeks geconverteerd naar een dynamische waarde.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Alle expressies pad waarmee u bladeren in een dynamische-object opgesomd. 

**Geeft als resultaat**

Een matrix van expressies pad.

**Voorbeelden**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Opmerking dat "[0]" geeft aan dat de aanwezigheid van een matrix, maar geeft niet de index die wordt gebruikt door een bepaald pad.

### <a name="zip"></a>ZIP

    zip(list1, list2, ...)

Combineert een aantal lijsten in een lijst met tuples.

* `list1...`: Een lijst met waarden

**Voorbeelden**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Namen

Namen kunnen maximaal 1024 tekens worden. Ze zijn hoofdlettergevoelig en mag letters, cijfers en onderstrepingstekens bevatten (`_`). 

Een naam met offerte ['... '] or [" ... "] andere tekens opnemen in of gebruik een trefwoord als een naam. Bijvoorbeeld:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["pad naar\\file\n\'x\'"] | Gebruik \ naar escape-tekens|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Geen Hiermee heft - \ is letterlijk|
|[@"\now& vervolgens\"] | |
|[waar] | Als een naam met behulp van een sleutelwoord language|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


