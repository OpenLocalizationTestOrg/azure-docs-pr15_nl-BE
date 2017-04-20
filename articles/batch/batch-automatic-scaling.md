<properties
    pageTitle="Automatisch schalen computerknooppunten in een Batch Azure toepassingen | Microsoft Azure"
    description="Inschakelen van automatisch schalen op een wolk toepassingen dynamisch aanpassen van het aantal knooppunten in de groep berekenen."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Schaal automatisch aanpassen in een Batch Azure toepassingen-computerknooppunten

Met automatisch schalen kan de Azure Batch-service dynamisch knooppunten toevoegt of verwijdert berekenen in een groep van toepassingen op basis van parameters die u definieert. Potentieel bespaart u veel tijd en geld doordat automatisch aanpassen van het bedrag van rekenkracht gebruikt door de toepassing--knooppunten toevoegen als uw taak van taak eisen en deze verwijderen wanneer ze verkleinen.

Inschakelen van automatisch schalen op een pool van computerknooppunten door te koppelen aan een *formule automatisch schalen* die u, zoals definieert met de [PoolOperations.EnableAutoScale] [ net_enableautoscale] -methode in de bibliotheek [Batch.NET](batch-dotnet-get-started.md) . De Batch-service vervolgens deze formule wordt het aantal knooppunten berekenen die nodig zijn voor het uitvoeren van uw werkbelasting te bepalen. Batch reageert op service metrische gegevens die regelmatig worden verzameld en past u het aantal knooppunten in de groep met een configureerbaar interval op basis van de formule berekenen.

U kunt automatische schaling wanneer een groep van toepassingen wordt gemaakt of op een bestaande toepassingen. Ook kunt u een bestaande formule in een pool die "automatisch schalen' ingeschakeld. Batch biedt de mogelijkheid om formules te evalueren voordat ze toewijzen aan groepen, evenals de status van automatisch schalen wordt uitgevoerd.

## <a name="automatic-scaling-formulas"></a>Automatische schaling formules

Een automatische schaling formule is een string-waarde die u opgeeft met een of meer instructies en is toegewezen aan een groep van toepassingen [autoScaleFormula] [ rest_autoscaleformula] element (Batch REST) of [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] eigenschap (Batch .NET). Wanneer de toegewezen aan een groep, gebruikt de service Batch formule bepalen het aantal doel-computerknooppunten in de groep voor het volgende interval van verwerking (intervallen later meer). De formule tekenreeks mag niet meer dan 8 KB groot, maximaal 100-instructies die worden gescheiden door puntkomma's, en kunnen bevatten, regeleinden en opmerkingen kunt toevoegen.

U kunt zien van automatische schaling formules met behulp van een Batch automatisch schalen 'taal'. Formule instructies zijn vrij gevormd expressies waarin zowel service gedefinieerde variabelen (variabelen die zijn gedefinieerd door de service Batch) en door de gebruiker gedefinieerde variabelen (variabelen die u opgeeft). Ze kunnen verschillende bewerkingen uitvoeren op deze waarden met behulp van de ingebouwde typen, operators en functies. Bijvoorbeeld kan een instructie er als volgt uit:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formules bevatten meestal meerdere instructies die bewerkingen uitvoeren op waarden die zijn verkregen in de vorige jaarrekening. Bijvoorbeeld eerst wij verkrijgen van een waarde voor `variable1`, deze vervolgens doorgeeft aan een functie voor het vullen van `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Bij deze instructies in de formule, is uw doel is om te komen tot een aantal computerknooppunten die de groep moet worden geschaald naar--het **doel** aantal **specifieke knooppunten**. Dit nummer mag niet hoger, lager of gelijk aan het aantal knooppunten in de groep. Batch wordt geëvalueerd als een groep van toepassingen automatisch schalen formule na een bepaald tijdsinterval ([Automatische schaling intervallen](#automatic-scaling-interval) worden hierna besproken). Vervolgens past het het doel aantal knooppunten in de groep met het nummer waarmee de formule automatisch schalen op het moment van de evaluatie.

Als een snelle voorbeeld deze formule twee regels automatisch schalen geeft aan dat het aantal knooppunten moet worden aangepast overeenkomstig het aantal actieve taken, met een maximum van 10-computerknooppunten:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

In de volgende secties van dit artikel bespreekt de verschillende diensten die uw formules automatisch schalen, inclusief variabelen, operators, bewerkingen en functies. Leert u diverse compute resource- en taakgegevens cijfers in Batch te verkrijgen. U kunt deze gegevens op intelligente wijze aanpassen van uw groep knooppunt tellen op basis van de resource en taak de status. Vervolgens leert u hoe u een formule maken en inschakelen van automatisch schalen op een groep met de REST van de partij en de API's van .NET. We voltooit met enkele formules van.

> [AZURE.IMPORTANT] Elke Batch Azure-account is beperkt tot een maximum aantal cores (en dus computerknooppunten) die kan worden gebruikt voor verwerking. De Batch-service maakt knooppunten tot die grens core. Daarom kan het doel aantal computerknooppunten die is opgegeven in een formule niet worden bereikt. [Quota's en beperkingen voor de Batch Azure service](batch-quota-limit.md) Zie voor informatie over het weergeven en uw account quota's te verhogen.

## <a name="variables"></a>Variabelen

U kunt zowel de **service worden gedefinieerd** en **door de gebruiker gedefinieerde** variabelen in uw formules automatisch schalen. De service gedefinieerde variabelen zijn ingebouwd in de Batch-service alleen-lezen zijn en sommige zijn alleen-lezen. Door de gebruiker gedefinieerde variabelen zijn variabelen die *u* definieert. In het van de twee regels voorbeeldformule hierboven `$TargetDedicated` is een service gedefinieerde variabele, terwijl `$averageActiveTaskCount` is een door de gebruiker gedefinieerde variabele.

De onderstaande tabellen weergeven alleen-lezen en alleen-lezen variabelen die zijn gedefinieerd door de Batch-service.

U kunt de **ophalen** en **instellen van** de waarden van deze variabelen service gedefinieerd voor het beheren van het aantal knooppunten in een groep berekenen:

| Lezen / schrijven service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $TargetDedicated | Het **doel** het aantal **computerknooppunten gereserveerd** voor de groep. Dit is het aantal computerknooppunten die de groep moet worden geschaald naar. Omdat een groep niet te bereiken doel aantal knooppunten is een getal 'target'. Dit kan gebeuren als het doel aantal knooppunten voordat de groep is het eerste doel bereikt opnieuw door een evaluatie van de daaropvolgende automatisch schalen wordt gewijzigd. Het kan ook gebeuren als een Batch account knooppunt of core quotum is bereikt voordat het doel aantal knooppunten wordt bereikt. |
| $NodeDeallocationOption | De actie die plaatsvindt wanneer computerknooppunten worden verwijderd uit een groep. Mogelijke waarden zijn:<ul><li>**requeue**--taken onmiddellijk beëindigd en weer in de wachtrij geplaatst, zodat ze opnieuw worden gepland.<li>**beëindigen**, taken onmiddellijk beëindigd en worden deze verwijderd uit de wachtrij.<li>**taskcompletion**--wachten op voor de actieve taken te voltooien en vervolgens het knooppunt uit de groep verwijderd.<li>**retaineddata**--wacht op alle lokale taak opgeslagen gegevens op het knooppunt dat moet worden opgeruimd voordat het knooppunt wordt verwijderd uit de groep.</ul> |

U kunt **u** de waarde van deze service gedefinieerde variabelen aan te passen die zijn gebaseerd op de maatstaven van de Batch-service:

| Alleen-lezen service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $CPUPercent | Het gemiddelde percentage CPU-gebruik. |
| $WallClockSeconds | Het aantal seconden dat is verbruikt. |
| $MemoryBytes | Het gemiddelde aantal megabytes gebruikt. |
| $DiskBytes | Het gemiddelde aantal GB gebruikt op de lokale schijven. |
| $DiskReadBytes | Het aantal bytes dat is gelezen. |
| $DiskWriteBytes | Het aantal bytes dat is geschreven. |
| $DiskReadOps | Het aantal schijfbewerkingen lezen. |
| $DiskWriteOps | De telling van de schijf schrijfbewerkingen worden uitgevoerd. |
| $NetworkInBytes | Het aantal binnenkomende bytes. |
| $NetworkOutBytes | Het aantal uitgaande bytes. |
| $SampleNodeCount | Het aantal knooppunten berekenen. |
| $ActiveTasks | Het aantal taken in een actieve status. |
| $RunningTasks | Het aantal taken uitgevoerd. |
| $PendingTasks | De som van $ActiveTasks en $RunningTasks. |
| $SucceededTasks | Het aantal taken dat is voltooid. |
| $FailedTasks | Het aantal taken dat is mislukt. |
| $CurrentDedicated | Het huidige aantal speciale computerknooppunten. |

> [AZURE.TIP] De alleen-lezen, service gedefinieerde variabelen die hierboven zijn weergegeven zijn *objecten* waarmee u verschillende methoden gebruiken om gegevens die zijn gekoppeld aan elk. Zie [Voorbeeldgegevens verkrijgen](#getsampledata) hieronder voor meer informatie.

## <a name="types"></a>Typen

Deze **typen** worden ondersteund in een formule.

- dubbel
- doubleVec
- doubleVecList
- tekenreeks
- timestamp, tijdstempel is een samengestelde structuur die de volgende leden bevat:

    - jaar
    - maand (1-12)
    - dag (1-31)
    - Weekday (in de notatie van een getal, bijvoorbeeld 1 voor maandag)
    - uur (in de 24-uurs notatie, bijvoorbeeld 13 betekent 1 uur)
    - minuut (00-59)
    - tweede (00-59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Bewerkingen

Deze **bewerkingen** zijn toegestaan voor de typen die hierboven zijn vermeld.

| Bewerking                             | Ondersteunde operators   | Resultaattype   |
| ------------------------------------- | --------------------- | ------------- |
| dubbele *operator* dubbel              | +, -, *, /            | dubbel            |
| dubbele *operator* timeinterval        | *                     | TimeInterval      |
| doubleVec *operator* dubbel           | +, -, *, /            | doubleVec         |
| doubleVec *operator* doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *operator* dubbel        | *, /                  | TimeInterval      |
| TimeInterval *operator* timeinterval  | +, -                  | TimeInterval      |
| TimeInterval *operator* tijdstempel     | +                     | tijdstempel         |
| tijdstempel *operator* timeinterval     | +                     | tijdstempel         |
| *operator* -tijdstempel timestamp        | -                     | TimeInterval      |
| dubbele *operator*                      | -, !                  | dubbel            |
| *operator*timeinterval                | -                     | TimeInterval      |
| dubbele *operator* dubbel              | < < =, ==, > =, >,! =  | dubbel            |
| string voor String- *operator*              | < < =, ==, > =, >,! =  | dubbel            |
| *operator* -tijdstempel timestamp        | < < =, ==, > =, >,! =  | dubbel            |
| TimeInterval *operator* timeinterval  | < < =, ==, > =, >,! =  | dubbel            |
| dubbele *operator* dubbel              | & &, & #124; & #124;      | dubbel            |

Bij het testen van een dubbel met een ternaire operator (`double ? statement1 : statement2`) niet-nul is **ingesteld op true**en nul is **ingesteld op false**.

## <a name="functions"></a>Functies

Deze vooraf gedefinieerde **functies** zijn beschikbaar voor gebruik bij het definiëren van een automatische schaling formule.

| Functie                          | Type retourwaarde   | Beschrijving
| --------------------------------- | ------------- | --------- |
| Avg(doubleVecList)                | dubbel        | Deze eigenschap retourneert de gemiddelde waarde van alle waarden in de doubleVecList.
| Len(doubleVecList)                | dubbel        | Deze eigenschap retourneert de lengte van de vector die is gemaakt op basis van de doubleVecList.
| LG(Double)                        | dubbel        | Deze eigenschap retourneert het logboek grondtal 2 van de dubbele.
| LG(doubleVecList)                 | doubleVec     | Deze eigenschap retourneert het logboek componentwise grondtal 2 van de doubleVecList. Een vec(double) moet expliciet worden doorgegeven voor de parameter. Anders wordt wordt de dubbele lg(double) versie uitgegaan.
| ln(Double)                        | dubbel        | Deze eigenschap retourneert een natuurlijk logaritme van het getal met dubbele precisie.
| ln(doubleVecList)                 | doubleVec     | Deze eigenschap retourneert het logboek componentwise grondtal 2 van de doubleVecList. Een vec(double) moet expliciet worden doorgegeven voor de parameter. Anders wordt wordt de dubbele lg(double) versie uitgegaan.
| log(Double)                       | dubbel        | Deze eigenschap retourneert het logboek grondtal 10 van de dubbele.
| log(doubleVecList)                | doubleVec     | Deze eigenschap retourneert het logboek componentwise grondtal 10 van de doubleVecList. Een vec(double) moet expliciet voor de dubbele één parameter worden doorgegeven. Anders wordt wordt de dubbele log(double) versie uitgegaan.
| Max(doubleVecList)                | dubbel        | Geeft als resultaat de maximumwaarde in de doubleVecList.
| min(doubleVecList)                | dubbel        | Geeft als resultaat de minimumwaarde in de doubleVecList.
| norm(doubleVecList)               | dubbel        | Deze eigenschap retourneert de twee-norm van de vector die is gemaakt op basis van de doubleVecList.
| percentiel (v doubleVec, double p) | dubbel        | Deze eigenschap retourneert het element percentiel van de vector v.
| ASELECT()                            | dubbel        | Deze eigenschap retourneert een willekeurige waarde tussen 0.0 en 1.0.
| Range(doubleVecList)              | dubbel        | Deze eigenschap retourneert het verschil tussen min en max waarden in de doubleVecList.
| Std(doubleVecList)                | dubbel        | Geeft als resultaat de standaarddeviatie van de steekproef van de waarden in de doubleVecList.
| Stop()                            |               | Evaluatie van de expressie autoscaling stopt.
| SUM(doubleVecList)                | dubbel        | Berekent de som van alle onderdelen van de doubleVecList.
| tijd (dateTime string = "")          | tijdstempel     | Deze eigenschap retourneert het tijdstempel van de huidige tijd als er geen parameters zijn doorgegeven, of het tijdstempel van de tekenreeks voor datum/tijd als deze wordt doorgegeven. DateTime ondersteunde indelingen zijn W3C-DTF- en RFC 1123.
| Val (v doubleVec, dubbele i)        | dubbel        | Deze eigenschap retourneert de waarde van het element dat zich op locatie i in vector v, met een begin-index van nul.

Enkele van de functies die worden beschreven in de bovenstaande tabel kan een lijst accepteren als argument. De door komma's gescheiden lijst is een combinatie van *dubbele* en *doubleVec*. Bijvoorbeeld:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

De *doubleVecList* wordt omgezet in een enkele *doubleVec* voor evaluatie. Bijvoorbeeld als `v = [1,2,3]`, vervolgens roepen `avg(v)` is gelijk aan het aanroepen `avg(1,2,3)`. Het aanroepen van `avg(v, 7)` is gelijk aan het aanroepen `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Voorbeeldgegevens verkrijgen

Automatisch schalen formules werken op metrische gegevens (monsters) die wordt geleverd door de Batch-service. Een formule vergroot of verkleind op basis van de waarden die deze van de service ontvangt groepsgrootte. De service gedefinieerde variabelen die hierboven zijn beschreven, zijn objecten die verschillende methoden bieden voor toegang tot gegevens die is gekoppeld aan het object. De volgende expressie geeft bijvoorbeeld een aanvraag voor het verkrijgen van de laatste vijf minuten van het CPU-gebruik:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Beschrijving |
| --- | --- |
| GetSample() | De `GetSample()` methode geeft als resultaat een vector voorbeelden.<br/><br/>Een voorbeeld is 30 seconden metrische gegevens. Met andere woorden, verkregen monsters elke 30 seconden. Maar zoals hieronder aangegeven, is er een vertraging tussen wanneer een monster wordt verzameld en dat deze beschikbaar is voor een formule. Als zodanig kunnen niet alle monsters voor een bepaalde periode beschikbaar voor evaluatie door een formule zijn.<ul><li>`doubleVec GetSample(double count)`<br/>Hiermee geeft u het aantal monsters te verkrijgen van de meest recente monsters die zijn verzameld.<br/><br/>`GetSample(1)`Deze eigenschap retourneert het laatste voorbeeld beschikbaar. Parameters zoals `$CPUPercent`, maar dit moet niet worden gebruikt omdat het onmogelijk is te weten *wanneer* het monster is verzameld. Kan het zijn recente of vanwege problemen met uw systeem, kan het veel ouder zijn. Het is beter in dat geval gebruik van een tijdsinterval, zoals hieronder wordt weergegeven.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Hiermee geeft u een periode voor het verzamelen van gegevens van de steekproef. Eventueel specificeert het ook het percentage van de monsters die in het opgegeven tijdsbestek beschikbaar moet zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`het resultaat 20 monsters als alle monsters gedurende de laatste tien minuten aanwezig in de geschiedenis van CPUPercent zijn. Als de laatste minuut van de geschiedenis niet beschikbaar is, echter zouden alleen 18 monsters worden geretourneerd. In dit geval:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`mislukte omdat slechts 90 procent van de monsters beschikbaar zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`zou slagen.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hiermee geeft u een periode voor het verzamelen van gegevens met een begintijd en eindtijd.<br/><br/>Zoals hierboven vermeld, is er een vertraging tussen wanneer een monster wordt verzameld en dat deze beschikbaar is voor een formule. Dit moet worden beschouwd als u werkt met de `GetSample` methode. Zie `GetSamplePercent` hieronder.|
| GetSamplePeriod() | De periode van de monsters die zijn genomen retourneert in een monster van historische gegevens. |
| Count() | Geeft als resultaat het totale aantal monsters in de metrische geschiedenis. |
| HistoryBeginTime() | Deze eigenschap retourneert het tijdstempel van het oudste monster van de beschikbare gegevens voor de metric. |
| GetSamplePercent() |Deze eigenschap retourneert het percentage van de monsters die beschikbaar voor een bepaald tijdsinterval zijn. Bijvoorbeeld:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Omdat de `GetSample` methode mislukt als het percentage van de monsters die als resultaat van minder dan de `samplePercent` opgeeft, kunt u de `GetSamplePercent` methode voor het eerst in de. Vervolgens kunt u een alternatieve actie monsters onvoldoende aanwezig zijn, zonder te stoppen, de automatische schaling evaluatie uitvoeren.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Samples, sample percentage en de methode *GetSample()*

De werking van de kern van een formule automatisch schalen is het verkrijgen van taak- en resourcegegevens metrische gegevens en pas vervolgens groepsgrootte op basis van die gegevens. Als zodanig is het belangrijk dat u een duidelijk beeld krijgt van de wisselwerking tussen de formules automatisch schalen en metrische gegevens of 'voorbeelden'.

**Voorbeelden**

De Batch-service periodiek neemt *monsters* van de taak- en resourcegegevens metrics en kan deze worden formules automatisch schalen. Deze monsters worden elke 30 seconden geregistreerd door de Batch-service. Er is echter meestal enkele latentie dat ervoor zorgt een vertraging dat tussen wanneer deze monsters zijn opgenomen en wanneer ze beschikbaar zijn gesteld voor (en kunnen worden gelezen door) de formules automatisch schalen. Ook als gevolg van verschillende factoren zoals het netwerk of andere problemen infrastructuur monsters mogelijk niet is geregistreerd voor een bepaald interval. Dit resulteert in 'ontbrekende' monsters.

**Percentage van de steekproef**

Wanneer `samplePercent` wordt doorgegeven aan de `GetSample()` methode of de `GetSamplePercent()` wordt opgeroepen, "percentage" verwijst naar een vergelijking tussen de totale *mogelijk* aantal monsters die door de service Batch worden geregistreerd en het aantal monsters die daadwerkelijk *beschikbaar* voor de formule automatisch schalen zijn.

We bekijken een interval van 10 minuten als voorbeeld. Omdat monsters worden vastgelegd om de 30 seconden, binnen een interval van 10 minuten is de maximale totale aantal monsters die zijn vastgelegd door de batchverwerking 20 monsters (2 per minuut). Echter door de inherente vertragingstijd van het mechanisme voor rapportage of sommige andere kwestie binnen de infrastructuur van Azure, kan er slechts 15 monsters die beschikbaar voor de formule automatisch schalen om te lezen zijn. Dit betekent dat, voor die periode 10 minuten slechts **75 procent** van het totale aantal monsters geregistreerd werkelijk beschikbaar is voor de formule.

**GetSample() en een voorbeeld van bereiken**

De formules automatisch schalen gaan worden vergroten en verkleinen van uw toepassingen--knooppunten toevoegen of verwijderen van knooppunten. Knooppunten kost u geld, wilt u ervoor zorgen dat uw formules gebruikt een intelligente analysemethode die is gebaseerd op voldoende gegevens. Daarom raden wij u een analyse-volgen van trends gebruiken in uw formules. Dit type wordt vergroot of verkleind op basis van een *bereik* van de verzamelde monsters van uw toepassingen.

Gebruik hiervoor `GetSample(interval look-back start, interval look-back end)` om terug te keren een **vector** van monsters:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Als de bovenstaande regel wordt geëvalueerd door de partij, wordt een bereik van monsters als vector van waarden geretourneerd. Bijvoorbeeld:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Nadat u de vector monsters hebt verzameld, u kunt functies zoals `min()`, `max()`, en `avg()` zinvolle waarden afleiden uit het bereik opgehaald.

Voor extra beveiliging kunt u een formule-evaluatie *niet* forceren, als er minder dan een bepaald percentage van het monster beschikbaar is voor een bepaalde periode. Wanneer u een formule-evaluatie niet afdwingen, u de opdracht Batch te beëindigen verdere evaluatie van de formule als het opgegeven percentage van de monsters niet beschikbaar--geeft en geen groep grootte wordt gewijzigd. Als u een vereist percentage van de monsters voor de evaluatie te kunnen uitvoeren, opgeven als de derde parameter van `GetSample()`. Hier, wordt een eis van 75 procent van de monsters opgegeven:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Het is ook belangrijk, door de eerder genoemde vertraging in de beschikbaarheid van monster geeft altijd een periode met een look back begintijd die ouder is dan één minuut. Dit is omdat het duurt ongeveer één minuut voor monsters doorgeven via het systeem, dus monsters in het bereik `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` vaak niet meer beschikbaar. Nogmaals, kunt u de parameter percentage van `GetSample()` te dwingen een vereiste monster bepaald percentage.

> [AZURE.IMPORTANT] We **raden ten zeerste aan** dat u *alleen *consoleberichten ** op `GetSample(1)` in uw formules automatisch schalen **. Dit komt omdat `GetSample(1)` zegt in feite naar de Batch-service 'Geef me het laatste voorbeeld hebt, ongeacht hoe lang geleden is goed." Omdat één sample is en is het mogelijk een oudere monster, het niet mogelijk vertegenwoordiger van de grotere afbeelding van de taak of resource staat. Als u `GetSample(1)`, zorg ervoor dat het deel van een groter overzicht en niet de enige plaats die afhankelijk zijn uitmaakt van de formule.

## <a name="metrics"></a>Statistieken

Kunt u statistieken voor **resource** - en **Taakgegevens** wanneer u een formule definieert. U aanpassen het nummer van het doel van specifieke knooppunten in de groep op basis van de metrische gegevens verkrijgen en evalueren. Zie de sectie [variabelen](#variables) hierboven voor meer informatie over elke maateenheid.

<table>
  <tr>
    <th>Metric</th>
    <th>Beschrijving</th>
  </tr>
  <tr>
    <td><b>Resource</b></td>
    <td><p><b>Resource metrics</b> zijn gebaseerd op het gebruik van CPU, bandbreedte en geheugen van computerknooppunten als het aantal knooppunten.</p>
        <p> Deze service gedefinieerde variabelen zijn handig voor correcties op basis van het aantal knooppunten:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Deze service gedefinieerde variabelen zijn handig voor correcties op basis van het Resourcegebruik knooppunt:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Taak</b></td>
    <td><p><b>Taak metrics</b> zijn gebaseerd op de status van taken, zoals het actief, in behandeling, en voltooid. De volgende service gedefinieerde variabelen zijn handig voor groepsgrootte correcties op basis van gegevens van de taak:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Schrijven van een formule automatisch schalen

U een formule automatisch schalen is samengesteld door de vorming van instructies voor het gebruik van de bovenstaande onderdelen vervolgens combineren in een formule voor volledige instructies. In deze sectie maken we een voorbeeld automatisch schalen formule die enkele concrete schaal beslissingen kunt uitvoeren.

Eerst gaan we de vereisten gedefinieerd voor onze nieuwe formule voor automatisch schalen. De formule moet:

1. **Vergroot** het aantal doel computerknooppunten in een groep als het CPU-gebruik hoog is.
2. **Verlagen** het doel aantal computerknooppunten in een groep als het CPU-gebruik laag is.
3. Altijd het **maximum** aantal knooppunten beperken tot 400.

*Verhoog* het aantal knooppunten in een hoog CPU-gebruik, definiëren we de instructie die u invult op een door de gebruiker gedefinieerde variabele (`$totalNodes`) met een waarde die is 110 procent van het huidige target aantal knooppunten, maar alleen als de minimale gemiddelde CPU-gebruik tijdens de laatste 10 minuten is meer dan 70 procent. Anders gebruikt u de huidige waarde toegewezen.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Verlaagt* het aantal knooppunten bij laag CPU-gebruik, de volgende instructie in onze formule stelt dezelfde `$totalNodes` variabele 90 procent van het huidige target aantal knooppunten als de gemiddelde CPU-gebruik in de afgelopen 60 minuten onder 20 procent was. Anders gebruikt u de huidige waarde van `$totalNodes` die we in het bovenstaande overzicht gevuld.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Nu het aantal doel specifieke computerknooppunten met een **maximum** van 400 beperken:

```
$TargetDedicated = min(400, $totalNodes)
```

Hier is de volledige formule:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Een ingeschakelde automatisch schalen van toepassingen maken

Als u wilt een nieuwe groep maken met autoscaling is ingeschakeld, kunt u een van de volgende technieken:

**Batch .NET**

1. De groep met [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx)maken.
1. De eigenschap [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) ingesteld op `true`.
1. Stel de eigenschap [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) met de formule automatisch schalen.
1. (Optioneel) De eigenschap [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) ingesteld (de standaardwaarde is 15 minuten).
1. De groep met [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) of [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx)worden doorgevoerd.

**Batch REST API**

* [Een groep aan een account toevoegen](https://msdn.microsoft.com/library/azure/dn820174.aspx): Geef de `enableAutoScale` en `autoScaleFormula` -elementen in uw aanvraag REST API voor het configureren van automatische schaling voor een groep van toepassingen wanneer u deze maakt.

Het volgende codefragment maakt een ingeschakelde automatisch schalen van toepassingen met behulp van de [Batch.NET] [ net_api] bibliotheek. De groep van toepassingen automatisch schalen formule wordt het doel aantal knooppunten op 5 op maandag 1 op alle andere dagen van de week. Het [interval voor automatische schaling](#automatic-scaling-interval) is ingesteld op 30 minuten. In deze en de andere C# fragmenten in dit artikel, "myBatchClient" is een goed geïnitialiseerd exemplaar van de [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Naast de Batch REST API en SDK voor .NET, kunt u een van de andere [Partij SDK's](batch-technical-overview.md#batch-development-apis), [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md)en de [Batch CLI](batch-cli-get-started.md) werken met autoscaling.

> [AZURE.IMPORTANT] Bij het maken van een groep automatisch schalen is ingeschakeld, moet u **niet** geven de `targetDedicated` parameter. Ook als u handmatig het formaat van een ingeschakelde automatisch schalen van toepassingen (bijvoorbeeld met [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), moet u eerst **uitschakelen** automatische schaling op de groep, vervolgens het formaat.

### <a name="automatic-scaling-interval"></a>Interval voor automatische schaling

Standaard past de Batch service groepsgrootte volgens de formule automatisch schalen om de **15 minuten**. Dit interval kan worden geconfigureerd, echter met behulp van de volgende eigenschappen:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

Het minimale interval is vijf minuten en de maximumwaarde is 168 uur. Als u een interval buiten dit bereik opgeeft, retourneert de Batch-service een fout Ongeldige aanvragen (400).

> [AZURE.NOTE] AutoScaling is momenteel niet bedoeld om te reageren op wijzigingen in minder dan een minuut, maar veeleer is bedoeld om de grootte van uw groep als u een werkbelasting geleidelijk aanpassen.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Autoscaling op een bestaande toepassingen inschakelen

Als u met een bepaald aantal computerknooppunten met behulp van de parameter *targetDedicated* al een groep hebt gemaakt, kunt u nog steeds autoscaling op de groep. Elke Batch SDK biedt een bewerking 'inschakelen automatisch schalen', bijvoorbeeld:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (Batch .NET)
*  [Inschakelen van automatisch schalen op een groep] [ rest_enableautoscale] (REST API)

Wanneer u autoscaling op een bestaande toepassingen inschakelt, geldt het volgende:

* Als automatisch schalen is **uitgeschakeld** op de groep van toepassingen wanneer u de aanvraag "inschakelen automatisch schalen", opgeven u *moet* een geldige automatisch schalen formule wanneer u het verzoek verzendt. U kunt *desgewenst* een automatisch schalen evaluatie interval opgeven. Als u een interval opgeeft, wordt de standaardwaarde van 15 minuten gebruikt.

* Als automatisch schalen **ingeschakeld** op de groep is, kunt u een formule automatisch schalen of een interval van de evaluatie. U kunt beide eigenschappen niet weglaten.

  * Als u een nieuw interval voor automatisch schalen evaluatie opgeeft, wordt gestopt met het bestaande schema voor de evaluatie en een nieuw schema wordt gestart. De nieuwe planning is start de tijd waarop de aanvraag 'automatisch schalen inschakelen' is afgegeven.

  * Als u de formule automatisch schalen of evaluatie interval, de service Batch weglaat blijft de huidige waarde van deze instelling gebruiken.

> [AZURE.NOTE] Als een waarde is opgegeven voor de parameter *targetDedicated* als de groep is gemaakt, wordt deze genegeerd wanneer de automatische schaling formule wordt geëvalueerd.

Dit codefragment C# gebruikt de [Batchverwerking .NET] [ net_api] bibliotheek autoscaling op een bestaande toepassingen inschakelen:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Een formule automatisch schalen bijwerken

U gebruikt dezelfde "automatisch schalen inschakelen" aanvraag voor het *bijwerken* van de formule op een bestaande ingeschakelde automatisch schalen van toepassingen (bijvoorbeeld met [EnableAutoScale] [ net_enableautoscale] in Batch .NET). Er is geen speciale 'update automatisch schalen'-bewerking. Bijvoorbeeld als autoscaling is reeds ingeschakeld op de 'myexistingpool' wanneer u de volgende code wordt uitgevoerd, de formule automatisch schalen wordt vervangen door de inhoud van `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Het interval voor automatisch schalen

Als u met het bijwerken van een formule automatisch schalen, gebruikt u de dezelfde [EnableAutoScale] [ net_enableautoscale] methode om het interval automatisch schalen evaluatie van een bestaande ingeschakelde automatisch schalen van toepassingen te wijzigen. Bijvoorbeeld voor het automatisch schalen evaluatie-interval instelt op 60 minuten voor een groep die al automatisch schalen is ingeschakeld:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Evalueren van een formule automatisch schalen

U kunt een formule evalueren voordat u het toepast op een groep. Op deze manier kunt u uitvoeren een "test uitvoeren" van de formule om te zien hoe de overzichten evalueren voordat u de formule in productie genomen.

Voor het evalueren van een formule automatisch schalen, moet u de eerste **autoscaling inschakelen** op de groep met een **geldige formule**. Als u testen een formule in een groep die nog geen autoscaling is ingeschakeld wilt, kunt u de formule van één regel `$TargetDedicated = 0` wanneer u autoscaling eerst inschakelen. Gebruik vervolgens een van de volgende opties voor de evaluatie van de formule die u wilt testen:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) of [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Deze Batch .NET methoden moeten de ID van een bestaande toepassingen en een tekenreeks met de formule automatisch schalen te evalueren. Resultaten van de evaluaties zijn opgenomen in de geretourneerde [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) instantie.

* [Een automatische schaling Formule evalueren](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Geef de pool-ID in de URI en de formule automatisch schalen in het *autoScaleFormula* -element van het hoofdgedeelte van de aanvraag in deze aanvraag REST API. Het antwoord van de bewerking bevat foutinformatie die gerelateerd zijn aan de formule.

In deze [Batch.NET] [ net_api] codefragment, evalueren we een formule voor het toepast op de [CloudPool][net_cloudpool]. Als de groep geen autoscaling ingeschakeld, kunnen we het.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Geslaagde evaluatie van de formule in dit fragment resulteert in de volgende uitvoer:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informatie over het automatisch schalen wordt uitgevoerd

De formule wordt uitgevoerd zoals verwacht, zodat het beste controleert regelmatig de resultaten van de autoscaling 'runs' Batch wordt uitgevoerd op de pool. Een verwijzing naar de groep,, zorg (of vernieuwen) en bekijk de eigenschappen van de laatste automatisch schalen worden uitgevoerd.

De eigenschap [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) heeft in .NET Batch, verschillende eigenschappen bieden informatie over de meest recente automatische schaling uitvoeren uitgevoerd op de pool door de Batch-service.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

Verzoek [informatie over een groep van toepassingen](https://msdn.microsoft.com/library/dn820165.aspx) in de REST API, geeft als resultaat informatie over de groep, waaronder de laatste automatische schaling informatie in [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)uitgevoerd.

De volgende C# stukje code gebruikt de bibliotheek Batch .NET informatie afdrukken over de laatste autoscaling op 'myPool' van toepassingen worden uitgevoerd:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Voorbeeld van de uitvoer van het voorgaande fragment:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Formules automatisch schalen

Laten we eens enkele formules die pas de compute-bronnen in een groep van toepassingen op verschillende manieren weergeven.

### <a name="example-1-time-based-adjustment"></a>Voorbeeld 1: Op basis van tijd aanpassing

Misschien wilt u de grootte van de groep op basis van de dag van de week en tijd van de dag te verhogen of verlagen van het aantal knooppunten in de pool dienovereenkomstig aanpassen.

Deze formule wordt eerst de huidige tijd. Is het een dag (1-5) en binnen de werkuren (8 uur tot 6 uur), wordt het doelformaat van toepassingen is ingesteld op 20 knooppunten. Anders wordt ingesteld op 10 knooppunten.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Voorbeeld 2: Op basis van taken aanpassen

In dit voorbeeld wordt de groepsgrootte aangepast op basis van het aantal taken in de wachtrij. Merk op dat zowel opmerkingen als regeleinden in formule tekenreeksen aanvaardbaar.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Voorbeeld 3: Boekhouding voor parallelle taken.

Dit is een ander voorbeeld dat wordt aangepast aan de grootte van de groep van toepassingen op basis van het aantal taken. Deze formule ook rekening gehouden met de [MaxTasksPerComputeNode] [ net_maxtasks] -waarde die is ingesteld voor de groep. Dit is vooral handig in situaties waarbij [uitvoering van parallelle taken](batch-parallel-node-tasks.md) is ingeschakeld voor uw groep.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Voorbeeld 4: Instellen van de grootte van een eerste groep van toepassingen

In dit voorbeeld ziet een C#-codefragment met een formule automatisch schalen die grootte van de groep worden ingesteld op een bepaald aantal knooppunten voor een eerste periode. En het past u de grootte van de groep van toepassingen op basis van het aantal actieve en actieve taken na de eerste periode is verstreken.

De formule in het volgende codefragment:

- Stelt de grootte van de eerste groep op vier knooppunten.
- Grootte van de groep niet aangepast binnen de eerste 10 minuten van de levenscyclus van de toepassingen.
- Na 10 minuten wordt de maximale waarde van het aantal actieve en actieve taken binnen de laatste 60 minuten.
  - Als beide waarden zijn 0 (hetgeen betekent dat er geen taken uitgevoerd of is actief in de laatste 60 minuten zijn), wordt de grootte van de groep ingesteld op 0.
  - Als de waarde groter dan nul is, wordt geen wijziging aangebracht.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Volgende stappen

* [Azure Batch maximaliseren berekenen Resourcegebruik met gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) bevat details over hoe u meerdere taken tegelijkertijd op de compute nodes in uw groep uitvoeren kunt. Deze functie kan naast autoscaling helpen te verlagen van de duur van de taak voor sommige werklasten, u geld besparen.

* Voor een andere donorlading efficiëntie, ervoor zorgen dat uw Batch toepassing query's in de Batch-service in de meest optimale manier. In [de Batch Azure service efficiënt Query](batch-efficient-list-queries.md)leert u de hoeveelheid gegevens die over het netwerk wanneer u een query naar de status van de mogelijk duizenden computerknooppunten of taken beperken.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
