<properties
    pageTitle="Zoekopdrachten registreren in logboek Analytics | Microsoft Azure"
    description="Logboek voor zoekopdrachten kunnen u combineren en correleren machine gegevens uit meerdere bronnen in uw omgeving."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Zoekopdrachten in Analytics logboek logboek

De kern van het logboek Analytics is de zoekfunctie logboek zodat u kunt combineren en correleren machine gegevens uit meerdere bronnen in uw omgeving. Oplossingen zijn ook aangedreven door logboek zoeken zodat je metrics gedraaid om een bepaald probleemgebied.

Op de zoekpagina kunt u een query maken en vervolgens wanneer u zoekt, u kunt de resultaten filteren door besturingselementen facet. U kunt ook geavanceerde zoekopdrachten te transformeren, filter en een rapport maken over de resultaten.

Algemene logboek zoekquery's weergegeven op de meeste oplossing's. U kunt overal in de console OMS Klik op tegels of analyseren andere opdrachten voor het weergeven van details over het item met behulp van de zoekfunctie.

In deze zelfstudie bekijken we door middel van voorbeelden ter dekking van alle basisbenodigdheden wanneer u de zoekfunctie gebruiken.

We beginnen met eenvoudige, praktische voorbeelden en vervolgens op deze samenstellen zodat u een beter inzicht in de praktische use-cases over het gebruik van de syntaxis van de inzichten die u wilt ophalen uit de gegevens kunt ophalen.

Nadat u vertrouwd bent met zoektechnieken hebt, kunt u het [logboek Analytics melden zoeken verwijzing](log-analytics-search-reference.md)kunt bekijken.

## <a name="use-basic-filters"></a>Eenvoudige filters gebruiken

Het eerste wat u moet weten is dat het eerste deel van een zoekopdracht query voordat een "|" verticale sluisteken, is altijd een *filter*. U kunt dit beschouwen als een WHERE-component in TSQL--bepaalt *welke* subset van gegevens uit het gegevensarchief OMS pull. Zoeken in het gegevensarchief is grotendeels over het opgeven van de kenmerken van de gegevens die u uitpakken, wilt dus het is natuurlijk dat een query met de WHERE-component begint.

De meest eenvoudige filters die kunt u zijn *trefwoorden*, zoals 'fout' of 'time-out' of de naam van een computer. Deze typen van eenvoudige query's retourneren doorgaans de diverse vormen van gegevens in de resultaatset hetzelfde. Dit komt omdat het logboek Analytics heeft verschillende *typen* gegevens in het systeem.


### <a name="to-conduct-a-simple-search"></a>Een eenvoudige zoekopdracht uitvoeren
1. Klik op **Logboek zoeken**in de portal OMS.  
    ![tegel zoeken](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Typ in het queryveld `error` en klik op **Zoeken**.  
    ![Fout bij zoeken](./media/log-analytics-log-searches/oms-search-error.png)  
    De query voor `error` geretourneerd in de volgende afbeelding 12 **ConfigurationChange** records (die door de wijzigingen bijhouden wordt opgenomen), 18 **ConfigurationAlert** records (gegenereerd door de beoordeling van de configuratie) en 100.000 **gebeurtenis** records (verzameld door Log-beheer).   
    ![zoekresultaten](./media/log-analytics-log-searches/oms-search-results01.png)  

Deze filters zijn niet echt objectklassen typen. *Type* is slechts een tag of een eigenschap of een tekenreeks/naam/categorie, die is gekoppeld aan een gedeelte van de gegevens. Bepaalde documenten in het systeem worden gecodeerd als **Type: ConfigurationAlert** en sommige zijn gecodeerd als **Type: Perf**of **Type gebeurtenis:**, enzovoort. Elk zoekresultaat, document, record of post wordt alle ruwe eigenschappen en bijbehorende waarden voor elk van die gegevens van de gegevens en kunt u deze namen opgeven in het filter als u wilt dat alleen de records op te halen waar het veld heeft die waarde gegeven.

*Type* is gewoon een veld waarin alle records hebt, is het niet anders dan andere velden. Dit werd vastgesteld op basis van de waarde van het veld Soort. Deze record heeft een andere vorm of vorm. Incidenteel, **Type = Perf**, of **Type = gebeurtenis** is ook de syntaxis die u moet weten om te zoeken op prestatiegegevens of gebeurtenissen.

Kunt u een dubbele punt (:) of een gelijkteken (=) achter de veldnaam en vóór de waarde. **Type: gebeurtenis** en **Type gebeurtenis =** zijn in de zin equivalent, kunt u de gewenste stijl.

Dus, als het Type Perf = records hebben een veld met de naam 'CounterName', dan kunt u een query die lijkt op `Type=Perf CounterName="% Processor Time"`.

Hiermee krijgt u alleen de prestatiegegevens waarin de naam van het prestatiemeteritem 'percentage processortijd' is.

### <a name="to-search-for-processor-time-performance-data"></a>Om te zoeken naar de processor tijd prestatiegegevens
- Typ in het zoekveld-query`Type=Perf CounterName="% Processor Time"`

U kunt ook specifieker en **InstanceName = _ 'Totaal'** in de query is een Windows Prestatiemeter. U kunt ook een facet en een andere **waarde: veld**selecteren. Het filter wordt automatisch toegevoegd aan het filter in de query weergegeven. U kunt dit zien in de volgende afbeelding. Er wordt aangegeven waar u moet klikken om toe te voegen **InstanceName: '_Totaal'** aan de query zonder iets te typen.

![facet zoeken](./media/log-analytics-log-searches/oms-search-facet.png)

De query wordt nu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

In dit voorbeeld, u hoeft niet te geven **Type = Perf** om dit resultaat te bereiken. Omdat de velden CounterName en InstanceName alleen voor records van het Type bestaat = Perf, de query is specifiek om terug te keren dezelfde resultaten op als de vorige langer een:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Dit komt omdat de filters in de query worden geëvalueerd als *en* met elkaar. Effectief, meer velden die u toevoegt aan de criteria, krijgt u minder, specifieke en geraffineerde resultaten.

De query `Type=Event EventLog="Windows PowerShell"` is gelijk aan `Type=Event AND EventLog="Windows PowerShell"`. Het resultaat alle gebeurtenissen die zijn vastgelegd in en uit het gebeurtenislogboek van Windows PowerShell worden verzameld. Als u een filter meerdere keren de dezelfde facet herhaaldelijk te selecteren en vervolgens het probleem louter cosmetische is--kan deze elkaar op de zoekbalk, maar het resultaat nog steeds dezelfde resultaten op omdat de impliciete operator en er altijd is.

U kunt de impliciete operator en gemakkelijk met behulp van een exploitant niet expliciet omkeren. Bijvoorbeeld:

`Type:Event NOT(EventLog:"Windows PowerShell")`of een equivalent daarvan `Type=Event EventLog!="Windows PowerShell"` retourneren van alle gebeurtenissen van andere logboeken die niet het logboekbestand van Windows PowerShell.

Of u kunt andere Booleaanse operator, zoals 'OR'. De volgende query geeft als resultaat records waarvoor het gebeurtenislogboek een toepassing of het systeem is.

```
EventLog=Application OR EventLog=System
```

De bovenstaande query gebruikt, krijgt u posten voor beide logboeken in het hetzelfde resultaat.

Echter als u door het verlaten van de impliciete en plaats de of verwijdert, wordt vervolgens de volgende query niet produceren resultaten omdat er een logboekvermelding aan beide logboeken behoort niet. Elke logboekvermelding is op slechts één van de twee logboeken geschreven.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Extra filters gebruiken

De volgende query retourneert posten voor 2 gebeurtenislogboeken voor alle computers die u hebt verzonden gegevens.

```
EventLog=Application OR EventLog=System
```

![zoekresultaten](./media/log-analytics-log-searches/oms-search-results03.png)

Een van de velden of filters te selecteren wordt de query beperken met een bepaalde computer, met uitzondering van alle andere vakken. De resulterende query zou uitzien.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Die gelijk is aan de volgende, door de impliciete AND.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Elke query wordt in de volgende expliciete volgorde geëvalueerd. Opmerking het haakje.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Net als het veld gebeurtenislogboek kunt u alleen gegevens voor een aantal specifieke computers ophalen door toe te voegen of. Bijvoorbeeld:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Ook dit met de volgende query als resultaat **% CPU-tijd** voor alleen de geselecteerde twee computers.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Boole-operators
Met datetime en numerieke velden, kunt u zoeken naar waarden met *meer dan* *minder dan*, en *lager dan of gelijk*. U kunt eenvoudige operatoren, zoals >, <>, =, < =,! = in de zoekbalk query.


U kunt een specifiek gebeurtenislogboek zoeken voor een bepaalde periode. Bijvoorbeeld wordt de laatste 24 uur uitgedrukt met de volgende expressie mnemonische.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Zoeken met behulp van een Booleaanse operator
- Typ in het zoekveld-query`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![zoeken met Booleaanse](./media/log-analytics-log-searches/oms-search-boolean.png)

Hoewel u het tijdsinterval grafisch bepalen kunt en de meeste tijden u kunt dat doen, zijn er voordelen met inbegrip van een tijdfilter rechtstreeks in de query. Bijvoorbeeld, dit een prima oplossing met dashboards waar u de tijd voor elke tegel ongeacht de *algemene* tijd selector op de dashboardpagina kunt wijzigen. Zie voor meer informatie de [Tijd belangrijk is in het Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Bij het filteren op tijd, houd er rekening mee dat u resultaten voor het *snijpunt* van de twee krijgt perioden: die is opgegeven in de portal OMS (S1) en die is opgegeven in de query (S2).

![doorsnede](./media/log-analytics-log-searches/oms-search-intersection.png)

Dit betekent, als de perioden elkaar niet overlappen, bijvoorbeeld in de OMS portal waar u **deze week** en in de query u **vorige week definieert**, is er geen snijpunt en ontvangt u geen resultaten opgeleverd.

Vergelijkingsoperatoren gebruikt voor het TimeGenerated-veld zijn ook handig zijn in andere situaties. Bijvoorbeeld, met numerieke velden.

Bijvoorbeeld, gegeven dat waarschuwingen van de beoordeling van de configuratie van de volgende waarden voor prioriteit hebben:

- 0 = informatie
- 1 = waarschuwing
- 2 = kritiek

U kunt query's voor waarschuwingen en kritieke waarschuwingen en informatieve berichten met de volgende query ook uitsluiten:

```
Type=ConfigurationAlert  Severity>=1
```


Ook kunt u query's bereik. Dit betekent dat u kunt het bereik van het begin en einde van de waarden in een reeks. Bijvoorbeeld, als u wilt dat de gebeurtenissen van de Operations Manager gebeurtenislogboek waarin de gebeurtenis-id groter dan of gelijk aan 2100, maar niet groter dan 2199 is wilt vervolgens de volgende query retourneren.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] De syntaxis van het bereik moet u de dubbele punt (:) veld: scheidingsteken is en *niet* het gelijkteken (=). Zet de onderste en bovenste einde van het bereik tussen vierkante haken en gescheiden door twee punten (.).

## <a name="manipulate-search-results"></a>Zoekresultaten manipuleren

Wanneer u gegevens zoekt, zult u uw zoekopdracht verfijnen en hebben een goede niveau van controle over de resultaten. Wanneer de resultaten worden opgehaald, kunt u de opdrachten voor het omzetten kunt toepassen.

Opdrachten in het logboek Analytics zoekopdrachten *moet* volgen na de verticale sluisteken (|). Een filter moet altijd het eerste deel van een query-tekenreeks. Definieert de gegevensset waarmee u werkt en vervolgens "buizen" deze resultaten in een opdracht. Vervolgens kunt u de sluis extra opdrachten toe te voegen. Dit lijkt losjes op de Windows PowerShell-pijplijn.

In het algemeen wil het logboek Analytics zoektaal PowerShell stijl en richtlijnen zodat het vergelijkbaar is met de IT-professionals en het leerproces gemakkelijker volgen.

Opdrachten hebben namen van woorden, zodat u gemakkelijk kunt zien wat ze doen.  

### <a name="sort"></a>Sorteren

De sorteeropdracht kunt u de sorteervolgorde van een of meerdere velden definiëren. Zelfs als u geen, standaard, wordt een tijd aflopende volgorde afgedwongen. Er zijn altijd de meest recente resultaten bovenaan de zoekresultaten. Dit betekent dat wanneer u een zoekopdracht met uitvoert `Type=Event EventID=1234` wat echt wordt uitgevoerd voor u is:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Dat komt doordat het type ervaring die u bekend met in Logboeken bent. Bijvoorbeeld in de Windows-Logboeken.

Sorteren kunt u de manier wijzigen waarop de resultaten worden weergegeven. De volgende voorbeelden ziet hoe dit werkt.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


De eenvoudige voorbeelden tonen u hoe opdrachten werken--ze veranderen de vorm van de resultaten die het filter geretourneerd.

### <a name="limit-and-top"></a>Limiet en boven
Een andere minder bekende opdracht is de LIMIET. Limiet is een PowerShell-achtige verb. Limiet is functioneel gelijk aan de belangrijkste opdracht. De volgende query's retourneren dezelfde resultaten.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Om te zoeken met top
- Typ in het zoekveld-query`Type=Event EventID=600 | Top 1`   
    ![top zoeken](./media/log-analytics-log-searches/oms-search-top.png)

In de afbeelding, worden records met gebeurtenis-id 1000 358 = 600. De velden, facetten en filters aan de linkerkant tonen altijd informatie over de resultaten die *door het filter gedeelte* van de query, het gedeelte vóór een pipe-teken wordt. Het deelvenster **resultaten** retourneert alleen de meest recente resultaten van 1, omdat met de opdracht in de vorm en de resultaten verwerkt.

### <a name="select"></a>Selecteer

De SELECT-opdracht gedraagt zich net als de Select-Object in PowerShell. De methode retourneert de gefilterde resultaten die niet al hun oorspronkelijke eigenschappen. In plaats daarvan worden alleen de eigenschappen die u geselecteerd.

#### <a name="to-run-a-search-using-the-select-command"></a>Voor het uitvoeren van een zoekactie met de opdracht select

1. Typ in zoeken, `Type=Event` en klik op **Zoeken**.
2. Klik op **+ meer** in een van de resultaten de eigenschappen die de resultaten weergeven.
3. Sommige van deze expliciet selecteren en de query wordt gewijzigd in `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Selecteer zoeken](./media/log-analytics-log-searches/oms-search-select.png)

Dit is de opdracht is vooral handig als u wilt zoeken, uitvoer en kies alleen de gedeelten van de gegevens die echt van belang voor de exploratie, hetgeen vaak niet de volledige record. Dit is ook handig wanneer u records van verschillende typen hebben een *aantal* gemeenschappelijke kenmerken, maar niet *alle* eigenschappen worden gebruikt. De uitvoer genereren die natuurlijker op een tabel lijkt of geschikt voor geëxporteerd naar een CSV-bestand en vervolgens in Excel massaged.



## <a name="use-the-measure-command"></a>Gebruik de opdracht maatregel

MAATREGEL is een van de meest veelzijdige opdrachten in logboek Analytics zoekopdrachten. U kunt statistische *functies* toepassen op uw gegevens en de statistische resultaten gegroepeerd op een bepaald veld. Er zijn verschillende statistische functies die maatregel ondersteunt.

### <a name="measure-count"></a>Count() meten

De eerste statistische functie te werken met een van de eenvoudigste manieren om te begrijpen is de functie *count()* .

Resultaten van een zoekopdracht, zoals `Type=Event`, filters, ook wel genoemd facetten aan de linkerkant van de zoekresultaten weergeven. Een verdeling van de waarden van een bepaald veld voor de resultaten van zien de filters in de zoekopdracht uitgevoerd.

![het aantal maatregel zoeken](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Bijvoorbeeld in de bovenstaande afbeelding ziet u het veld van de **Computer** en het blijkt dat in bijna 739 duizend gebeurtenissen in de resultaten van de 68 unieke en verschillende waarden voor het veld van de **Computer** in deze records. De tegel toont uitsluitend de top 5 zijn de meest voorkomende 5 waarden die zijn geschreven in de velden van de **Computer** ), gesorteerd op het aantal documenten dat die specifieke waarde in dat veld bevatten. In de afbeelding ziet u dat-tussen de gebeurtenissen die bijna 369 duizend – 90 duizend afkomstig zijn van de computer OpsInsights04.contoso.com, duizend 83 van de computer DB03.contoso.com, enzovoort.


Wat gebeurt er als u wilt zien van alle waarden, omdat de tegel alleen de top 5 toont?

Dat is wat de opdracht maatregel kan doen met de functie count(). Deze functie geen parameters gebruikt. U opgeven hebt het veld waarop u wilt groeperen door – het veld van de **Computer** in dit geval:

`Type=Event | Measure count() by Computer`

![het aantal maatregel zoeken](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

**Computer** is echter alleen een veld dat wordt gebruikt *in* elk gedeelte van de gegevens – er zijn geen relationele databases betrokken en er is geen afzonderlijke **Computer** object overal. Alleen de waarden *in* de gegevens kunt beschrijven welke entiteit gegenereerd ze, en een aantal andere kenmerken en aspecten van de gegevens – vandaar de term *facet*. U kunt echter net zo goed op andere velden groeperen. Omdat de oorspronkelijke resultaten bijna 739 duizend gebeurtenissen die worden doorgesluisd naar de opdracht maatregel ook een veld met de naam van de **gebeurtenis-id hebben**, kunt u toepassen op dezelfde techniek als u wilt groeperen op een veld en een aantal gebeurtenissen met gebeurtenis-id:

```
Type=Event | Measure count() by EventID
```

Selecteer de eerste kolom als u niet geïnteresseerd bent in het aantal werkelijke records met een bepaalde waarde, maar in plaats daarvan als u alleen een lijst met de waarden zelf, u een *Selecteer* opdracht aan het einde daarvan en slechts toevoegen kunt:

```
Type=Event | Measure count() by EventID | Select EventID
```

Vervolgens kunt u vooraf de resultaten in de query sorteren en ingewikkeldere krijgen of hoeft u alleen op de kolommen in het raster te.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Om te zoeken met de telling van de maatregel

- Typ in het zoekveld-query`Type=Event | Measure count() by EventID`
- Append `| Select EventID` aan het einde van de query.
- Ten slotte toevoegen `| Sort EventID asc` aan het einde van de query.


Er zijn een paar belangrijke punten op te merken en te benadrukken:

Ten eerste zijn de weergegeven resultaten niet de oorspronkelijke raw resultaten meer. In plaats daarvan deze samengevoegde resultaten – zijn in feite de groepen van de resultaten. Dit is niet een probleem, maar u moet begrijpen dat u wilt communiceren met een heel andere vorm van gegevens die verschilt van de oorspronkelijke ruwe vorm die wordt gemaakt op elk gewenst moment als gevolg van de samenvoeging/statistische functie.

Ten tweede retourneert **maatregel tellen** momenteel alleen de top 100 van verschillende resultaten. Deze limiet geldt niet voor de andere statistische functies. Dus moet meestal u eerst een nauwkeurigere filter gebruiken om te zoeken naar specifieke artikelen voordat u count() maatregel toepast.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Gebruik de functies max en min met de opdracht maatregel

Er zijn verschillende scenario's waar **Max() maatregel** en de **Maatregel zijn** nuttig zijn. Echter, aangezien elke functie Tegengesteld laten we van elkaar zult Max() zien en u kunt experimenteren met zijn op uw eigen.

Als u een query voor beveiligingsgebeurtenissen uitvoert, hebben ze een eigenschap **Level** die kan variëren. Bijvoorbeeld:

```
Type=SecurityEvent
```

![zoeken eenheid count start](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Als u wilt bekijken, de hoogste waarde voor alle zekerheid kunt gebeurtenissen gegeven een gemeenschappelijke Computer, de groep door het veld, u

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![max maat-computer zoeken](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Deze wordt weergegeven dat voor de computers die bijvoorbeeld records **niveau** , de meeste van hen hebben van ten minste 8 niveau, veel had een niveau van 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![maximale zoektijd maatregel computer gegenereerd](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Deze functie werkt goed met getallen, maar ook in combinatie met de datum/tijd-velden. Het is nuttig om te controleren op de laatste of de meest recente tijdstempel voor elk deel van de gegevens voor elke computer geïndexeerd. Bijvoorbeeld: wanneer is de meest recente beveiligingsgebeurtenis gemeld voor elke computer?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Gebruik de functie avg met de opdracht maatregel

De Avg() van een statistische functie gebruikt in combinatie met de maatregel kunt u de gemiddelde waarde voor een veld en de resultaten groeperen op het veld dat dezelfde of andere berekenen. Dit is handig in een aantal gevallen, zoals de prestatiegegevens.

We beginnen met de prestatiegegevens. Houd er rekening mee dat OMS momenteel prestatiemeteritems voor zowel Windows als Linux machines verzamelt.

Als u wilt zoeken naar *alle* gegevens, is de meest eenvoudige query:

```
Type=Perf
```

![avg start zoeken](./media/log-analytics-log-searches/oms-search-avg01.png)

Het eerste wat u ziet is dat logboek Analytics u drie perspectieven ziet: lijst, waarin de werkelijke records weergegeven achter de grafieken; Tabel waarin een tabelweergave van gegevens van prestatiemeteritems; Metrics, geeft en grafieken voor de prestatiemeteritems.

In de afbeelding zijn er twee sets van velden gemarkeerd die het volgende aangeven:

- De eerste set geeft Windows Performance Counter naam, naam en exemplaarnaam in de query-filter. Dit zijn de velden die u waarschijnlijk meestal als facetten/filters gebruikt wordt
- **Tegenwaarde** is de werkelijke waarde van de teller. In dit voorbeeld wordt de waarde *75*.
- **TimeGenerated** is 12:51 in 24-uursnotatie.

Hier is een weergave van de gegevens in een grafiek.

![avg start zoeken](./media/log-analytics-log-searches/oms-search-avg02.png)

Na het lezen van informatie over de record vorm Perf en Lees over andere zoektechnieken, kunt u dit soort numerieke gegevens samenvoegen maatregel Avg().

Hier volgt een eenvoudig voorbeeld:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Gem samplevalue zoeken](./media/log-analytics-log-searches/oms-search-avg03.png)

In dit voorbeeld selecteert u de totale CPU-tijd prestaties teller en een gemiddelde per Computer. Als u beperken de resultaten voor alleen de laatste 6 uur wilt, kunt u het besturingselement tijd filter gebruiken of in uw query opgeeft als volgt:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Om te zoeken met de functie avg met de opdracht maatregel
- Typ in het vak Search query `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


U kunt statistische en relateren van gegevens *tussen* computers. Stel bijvoorbeeld dat er een set hosts in een bepaald soort bedrijf waar elk knooppunt is gelijk aan de andere een ze net hetzelfde werk en typ laden ongeveer moet worden verdeeld. U kunt de items die in een gaan met de volgende query uitvoeren en gemiddelden voor de gehele farm ophalen kan ophalen. U kunt starten met het kiezen van de computers met het volgende voorbeeld:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Nu u de computers hebt, u ook alleen wilt selecteren van twee prestatie-indicatoren (KPI's): % CPU-gebruik en % vrije ruimte op de schijf. Dus dat deel van de query als volgt uit:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

U kunt nu toevoegen computers en items met het volgende voorbeeld:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Hebt u een zeer specifieke selectie kan **maatregel Avg()** retourneert de opdracht het gemiddelde niet door de computer, maar in de farm, door te groeperen door CounterName. Bijvoorbeeld:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Dit biedt u een handige compacte weergave van een aantal KPI's van uw omgeving.

![Search-avg groeperen](./media/log-analytics-log-searches/oms-search-avg04.png)


U kunt eenvoudig de zoekopdracht in een dashboard. Zo kan u de zoekopdracht opslaan en een dashboard maken vanuit deze *KPI's Web-Farm*genoemd. Zie voor meer informatie over het gebruik van dashboards, [maken een aangepast dashboard in logboek Analytics](log-analytics-dashboards.md).

![avg-dashboard zoeken](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>De functie SOM gebruiken met de opdracht maatregel

De functie SOM is vergelijkbaar met andere functies van de opdracht van de maatregel. Hier ziet u een voorbeeld over het gebruik van de functie SOM op [W3C IIS Logs zoeken in Microsoft Azure operationele inzichten](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

U kunt Max() en zijn met getallen, datums en tijden en tekenreeksen. Tekenreeksen, ze alfabetisch worden gesorteerd en u krijgt het eerste en laatste.

U kunt bladwijzers echter niet gebruiken met iets anders dan de numerieke velden. Dit geldt ook voor Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Gebruik de functie percentile met de opdracht maatregel

De functie percentile is vergelijkbaar met Avg() of bladwijzers dat u deze alleen voor numerieke velden gebruiken kunt. U kunt percentiel tussen 1 en 99 in een numeriek veld. U kunt ook **percentiel** - en **pct** -opdrachten gebruiken. Hier volgen enkele voorbeelden:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Gebruiken waar u de opdracht

De indien opdracht als een filter werkt, maar deze kan worden toegepast in de pijplijn samengevoegde resultaten die zijn verkregen door een opdracht die maatregel – nog verder filteren in plaats van onbewerkte resultaten die worden gefilterd op aan het begin van een query.

Bijvoorbeeld:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

U kunt een andere pijp toevoegen "|" teken en waar u de opdracht om alleen computers waarvan de gemiddelde CPU hoger dan 80%, met het volgende voorbeeld is:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Als u vertrouwd met Microsoft System Center - Operations Manager bent, kunt u zien waar u de opdracht in termen van management pack. Als het voorbeeld van een regel, het eerste deel van de query op de gegevensbron zou zijn en waar u de opdracht zou zijn van de voorwaarde-detectie.

U kunt de query als een tegel in **Mijn Dashboard**, als een monitor van sorteren, zien wanneer de computer CPU's zijn te weinig gebruikte. Zie voor meer informatie over dashboards, [maken een aangepast dashboard in logboek Analytics](log-analytics-dashboards.md). U kunt ook maken en gebruiken van dashboards met behulp van de mobiele app. Zie [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)voor meer informatie. In de onderste twee tegels van de volgende afbeelding ziet u het beeldscherm een lijst weergegeven als een getal. In feite, wilt u altijd het nummer nul en de lijst leeg. In andere gevallen geeft een waarschuwing voorwaarde aan. Indien nodig, kunt u deze bekijken op welke computers staan onder druk.

![mobiele dashboard](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>De operator in gebruiken

De operator *IN* samen met de *NOT IN* kunt u subsearches zoeken en opnemen van een andere zoekopdracht als argument worden gebruikt. Ze bevinden zich in de accolades {} binnen een andere *primaire* of *buitenste* zoeken. Het resultaat van een subsearch, vaak een lijst van de verschillende resultaten, wordt vervolgens gebruikt als een argument in het hoofdzoekveld.

Subsearches kunt u subsets van gegevens dat u beschrijft kan niet rechtstreeks in een zoekexpressie, maar die kunnen worden gegenereerd vanuit een zoekopdracht overeenkomen. Bijvoorbeeld als u geïnteresseerd bent in alle gebeurtenissen van *ontbrekende beveiligingsupdates computers*zoeken met een zoekopdracht, moet u voor het ontwerpen van een subsearch waarmee eerst die *ontbrekende beveiligingsupdates computers* voordat gebeurtenissen die deel uitmaken van deze hosts worden gevonden.

U kan dus express *momenteel ontbrekende vereiste beveiligingsupdates computers* met de volgende query:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![IN het voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Wanneer u de lijst hebt, kunt u de zoekopdracht als een interne zoekopdracht om de lijst met computers in een buitenste (primaire) zoeken die naar gebeurtenissen voor deze computers zoeken. Hiervoor zoeken binnen accolades omsluiten en voeding van de resultaten als mogelijke waarden voor een filterveld in de buitenste zoeken met behulp van de operator. De query zou vergelijkbaar zijn met:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![IN het voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Ook de aankondiging de tijdfilter gebruikt in de binnenste zoeken omdat het System Update Assessment een momentopname van alle computers elke 24 uur maakt. Kunt u de inner query meer licht en nauwkeurig door te zoeken op slechts een dag. De buitenste zoeken in plaats daarvan gebruikt de tijdzone in de gebruikersinterface ophalen van gebeurtenissen van de laatste 7 dagen. [Boole-operators](#boolean-operators) Zie voor meer informatie over operatoren tijd.

Omdat u eigenlijk alleen gebruiken de resultaten van de binnenste zoeken als een filter waarde voor de buitenste, kunt u de opdrachten in het buitenste zoeken nog steeds toepassen. U kunt bijvoorbeeld nog steeds deze gebeurtenissen met een andere maatregel opdracht groeperen:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![IN het voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in03-revised.png)


U wilt in het algemeen de inner query snel uitvoeren omdat logboek Analytics time-outs voor de service voor deze heeft en een kleine hoeveelheid resultaten retourneren. Als de inner query meer resultaten oplevert, de lijst met resultaten afgekapt, die zou kunnen veroorzaken met het buitenste zoeken onjuiste resultaten oplevert.

Een andere regel is dat het binnenste zoeken op dit moment moet *geaggregeerde* resultaten opleveren. Met andere woorden, moet het bevatten een *maatregel* , opdracht; u kan niet op dit moment rauwe resultaten in een buitenste zoeken feed.

Ook kan er slechts één IN-operator en het laatste filter dat in de query moeten worden. Meerdere IN operatoren geen of zou – Hiermee in feite wordt voorkomen dat meerdere subsearches uitgevoerd: het belangrijkste is dat slechts één sub/binnenste zoeken is mogelijk voor elke zoekopdracht outer.

IN vele soorten gecorreleerde zoekopdrachten kan zelfs met deze limieten, en kunt u definiëren of een vergelijkbare groepen, zoals computers, gebruikers of bestanden – ongeacht de velden in uw gegevens bevatten. Hier vindt u meer voorbeelden:

**Alle updates ontbreekt op computers waarop de instelling van de functie Automatische updates is uitgeschakeld**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Alle fouten van de computers met SQL Server (= waar SQL-analyse is uitgevoerd)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Alle beveiligingsgebeurtenissen van computers die domeincontrollers (= waar AD beoordeling is uitgevoerd)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Welke andere accounts op dezelfde computers waarop BACONLAND\jochan account heeft aangemeld bent aangemeld?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Gebruik de opdracht verschillen

Zoals de naam al aangeeft, levert deze opdracht een lijst met verschillende waarden voor een veld. Het is verrassend eenvoudige, maar zeer nuttig. Hetzelfde kan worden bereikt met maatregel count() opdracht ook, zoals hieronder wordt weergegeven.

```
Type=Event | Measure count() by Computer
```

![Voorbeeld van de opdracht verschillen zoeken](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Als u geïnteresseerd bent in is echter een lijst van verschillende waarden en niet het aantal documenten met waarden en vervolgens DISTINCT kan bieden helderder en beter leesbaar, uitvoer en kortere-syntaxis, zoals hieronder wordt weergegeven.

```
Type=Event | Distinct Computer
```
![Voorbeeld van de opdracht verschillen zoeken](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>De functie countdistinct gebruiken met de opdracht maatregel
De functie countdistinct telt het aantal unieke waarden in elke groep. Het kan bijvoorbeeld worden gebruikt om het aantal unieke computers rapportage voor elk Type:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Gebruik de opdracht maatregel interval
Met in de buurt van real-time prestatiegegevens verzamelen, te verzamelen en visualiseren van de prestatiemeteritems in logboek Analytics. De query **Type: Perf** duizenden metrieke grafieken retourneert eenvoudig in te voeren op basis van het aantal items en servers in uw omgeving logboek Analytics. Met metrische aggregatie op verzoek, kunt u de algemene parameters in uw omgeving op een hoog niveau en diepgaande Kennismaking naar meer gedetailleerde gegevens als u wilt bekijken.

Stel dat u wilt weten wat de gemiddelde CPU op alle computers. Kijken naar de gemiddelde CPU voor elke computer mogelijk niet handig omdat resultaten ophalen uitgevlakt. Als u wilt zoeken naar meer informatie, kunt u uw resultaat een kleiner venster segmenten gedownload, en uiterlijk in een tijdreeks samenvoegen over verschillende dimensies. Bijvoorbeeld, kunt u uitvoeren het gemiddelde uurtarief van CPU-gebruik op alle computers als volgt:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![gemiddelde interval maatregel](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Standaard wordt deze resultaten worden weergegeven in een lijngrafiek meerdere reeks interactieve.  Deze grafiek ondersteunt reeks uitschakelen (met de y-as schaling aanpassen), zoomen en laten rusten.  De tabeloptie is nog steeds beschikbaar voor het weergeven van onbewerkte gegevens indien nodig.

U kunt ook andere velden groeperen. Ik zoek op alle items voor een specifieke computer % en ik wil weten wat het uurloon 70 percentielen van elk item wordt in dit voorbeeld:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Één ding om te weten is dat deze query's niet beperkt tot prestatiemeteritems zijn. U kunt ze toepassen op een metric. In dit voorbeeld zoek ik op W3C, IIS-logboeken. Ik wil weten wat de maximale tijd een interval van 5 minuten neemt voor het verwerken van elke aanvraag is:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Meerdere aggregaten in één query gebruiken
In een opdracht maatregel kunt u meerdere samengestelde componenten.  Elk kan afzonderlijk alias worden.  Als er geen alias is opgegeven de naam van het resulterende veld worden de statistische functie die is gebruikt (dat wil zeggen "avg(CounterValue)" voor avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Hier volgt een voorbeeld:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het logboek zoeken:

- Gebruik [aangepaste velden in Analytics logboek](log-analytics-custom-fields.md) logboek zoekopdrachten uitbreiden.
- Bekijk het [logboek Analytics melden verwijzing zoeken](log-analytics-search-reference.md) om alle facetten beschikbaar in logboekbestand Analytics zoeken en weer te geven.
