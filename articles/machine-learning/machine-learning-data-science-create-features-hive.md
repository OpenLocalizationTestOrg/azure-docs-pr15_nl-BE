<properties
    pageTitle="Functies voor gegevens in een component-query's met Hadoop-cluster maken | Microsoft Azure"
    description="Voorbeelden van query's component die functies met gegevens die zijn opgeslagen in een cluster Azure HDInsight Hadoop maakt."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Functies voor gegevens in een Hadoop-cluster met behulp van component query's maken

Dit document bevat functies voor gegevens die zijn opgeslagen in een component-query's met Azure HDInsight Hadoop-cluster maken. Deze component query's gebruik ingesloten component gebruiker gedefinieerde functies (UDF's), de scripts die worden geleverd.

De bewerkingen die nodig zijn voor het maken van functies is geheugenintensief. De prestaties van query's component meer kritiek in dergelijke gevallen wordt en kan worden verbeterd door het afstemmen van bepaalde parameters. Het afstemmen van deze parameters wordt besproken in de laatste sectie.

Voorbeelden van de query's die worden weergegeven zijn specifiek voor de [RDAM Taxi rit gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar in [de bibliotheek Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebben gegevensschema opgegeven en klaar om te worden ingediend om uit te voeren. Parameters die gebruikers afstemmen kunnen, zodat de prestaties van component-query's kunnen worden verbeterd worden ook besproken in de laatste sectie.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Dit **menu** bevat koppelingen naar onderwerpen over het maken van functies voor gegevens in diverse omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u beschikt over:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, ziet u [een opslag Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Een aangepaste Hadoop cluster met de service HDInsight ingericht.  Als u instructies, Zie [Aanpassen Azure HDInsight Hadoop Clusters voor geavanceerde Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* De gegevens is geüpload naar de tabellen van de component in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, voert u [component tabellen maken en laden gegevens](machine-learning-data-science-move-hive-tables.md) eerst uploaden van gegevens aan tabellen component.
* Externe toegang tot het cluster ingeschakeld. Zie [toegang tot het hoofd van Hadoop Cluster knooppunt](machine-learning-data-science-customize-hadoop-cluster.md#headnode)als u instructies nodig hebt.


##<a name="hive-featureengineering"></a>Functie voor het genereren van

In dit gedeelte worden enkele voorbeelden van de manieren waarin functies kunnen genereren met behulp van component query's beschreven. Nadat u extra functies hebt gegenereerd, kunt u kolommen toevoegen aan de bestaande tabel of maak een nieuwe tabel met de primaire sleutel, die vervolgens kan worden gekoppeld met de oorspronkelijke tabel en extra functies. Hier zijn de voorbeelden:

1. [Functie genereren op basis van frequentie](#hive-frequencyfeature)
2. [Risico's van het bestaan van de variabelen in binaire indeling.](#hive-riskfeature)
3. [Functies van Datetime Field uitpakken](#hive-datefeatures)
4. [Functies van het tekstveld ophalen](#hive-textfeatures)
5. [Berekenen van de afstand tussen de GPS-coördinaten](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Functie genereren op basis van frequentie

Is het vaak handig om de frequenties van de niveaus van een variabele bestaan of de frequenties van bepaalde combinaties van de niveaus van meerdere bestaan variabelen te berekenen. Gebruikers kunnen het volgende script gebruiken voor het berekenen van deze frequenties:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Risico's van het bestaan van de variabelen in binaire indeling.

In binaire indeling moeten we niet-numerieke bestaan variabelen omzetten in numerieke functies wanneer modellen worden gebruikt alleen numerieke functies. Dit wordt gedaan door elke niet-numerieke niveau te vervangen door een numerieke risico. In deze sectie zien we sommige algemene component query's die de waarden van de risico's (logboek kans) van een variabele bestaan berekenen.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

In dit voorbeeld worden variabelen `smooth_param1` en `smooth_param2` zijn ingesteld voor een vlotte de risico-waarden berekend op basis van de gegevens. Risico's hebben een bereik tussen - INF -bestand en een INF-bestand. Een risico's > 0 geeft aan dat de kans dat het doel gelijk aan 1 is groter dan 0,5 is.

Na het risico wordt tabel berekend, wordt gebruikers kunnen risico waarden toewijzen aan een tabel door deze samen te voegen met de tabel risico. De component worden query is opgegeven in de vorige sectie.

###<a name="hive-datefeatures"></a>Functies van Datetime Fields uitpakken

Component wordt geleverd met een UDF's voor het verwerken van datum/tijd-velden. In de component, de standaardnotatie voor datum/tijd is ' jjjj-MM-dd-00:00:00 ' ('01-01-1970 12:21:32 ' bijvoorbeeld). We zien in deze sectie, voorbeelden die de dag van een maand, de maand van een datum/tijd-veld worden opgehaald en andere voorbeelden die een tekenreeks voor datum/tijd in een indeling converteren, andere dan de standaardnotatie voor een datum/tijd-tekenreeks opmaken in.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Deze component query wordt ervan uitgegaan dat de *& #60; datum/tijd-veld >* in de standaardnotatie voor datum/tijd is.

Als u een datum/tijd-veld niet in de standaardnotatie is, moet u eerst het veld Datum/tijd naar Unix tijdstempel converteren en vervolgens het Unix tijdstempel te converteren naar een datetime-tekenreeks die in de standaardindeling is. Wanneer de datum/tijd in standaard indeling is, kunnen gebruikers de ingesloten datetime UDF's uitpakken functies toepassen.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

In deze query als de *& #60; datum/tijd-veld >* heeft het patroon als *26-03/2015 12:04:39*, de *' & #60; patroon van het datum/tijd-veld >'* moeten worden `'MM/dd/yyyy HH:mm:ss'`. Als u wilt testen, kunnen gebruikers uitvoeren.

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

De *hivesampletable* in deze query is standaard geïnstalleerd op alle clusters van Azure HDInsight Hadoop standaard wanneer de clusters zijn ingericht.


###<a name="hive-textfeatures"></a>Functies van tekstvelden uitpakken

Wanneer de component-tabel een tekstveld met een tekenreeks van woorden die worden gescheiden door spaties is, wordt de volgende query de lengte van de tekenreeks en het aantal woorden in de tekenreeks.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Berekenen van de afstand tussen de sets met GPS-coördinaten

De query in deze sectie gegeven kan rechtstreeks worden toegepast in de RDAM Taxi rit gegevens. Het doel van deze query is het toepassen van een ingesloten wiskundige functies in de component voor het genereren van functies weergeven.

De velden die worden gebruikt in deze query worden de GPS-coördinaten van locaties voor afhalen en dropoff, met de naam *ophalen\_lengte*, *pickup\_latitude*, *dropoff\_lengte*, en *dropoff\_latitude*. De query's die de rechtstreekse afstand tussen de coördinaten ophalen en dropoff berekenen zijn:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

De wiskundige vergelijkingen die voor het berekenen van de afstand tussen twee GPS-coördinaten vindt u op de site <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">MovableType Scripts</a> , geschreven door Peter Lapisu. In zijn Javascript, de functie `toRad()` is gewoon *lat_or_lon*pi/180*, die graden naar radialen converteert. Hier *lat_or_lon * is de breedtegraad of lengtegraad. Aangezien de component biedt niet de functie `atan2`, maar de functie biedt `atan`, de `atan2` functie wordt geïmplementeerd door `atan` functie in de bovenstaande component query met behulp van de definitie in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Werkruimte maken](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Een volledige lijst van ingesloten UDF's u in de sectie **Ingebouwde functies** op de <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">component Apache wiki vindt</a>-component).  

## <a name="tuning"></a>Geavanceerde onderwerpen: Tune component Parameters Query snelheid verbeteren

De standaardinstellingen van de parameter van het onderdeel cluster mogelijk niet geschikt is voor de component query's en de gegevens die de query's worden verwerkt. In dit gedeelte bespreken we enkele parameters die gebruikers kunnen afstemmen de prestaties van query's component verbeteren. Gebruikers moeten de parameter tuning query's voordat de query's van de verwerking van gegevens toevoegen.

1. **Java heap ruimte**: voor query's met grote gegevenssets te koppelen of lange records verwerkt, **heap ruimte tekort** is een van de algemene fout. Dit kan worden aangepast door de parameters *mapreduce.map.java.opts* en *mapreduce.task.io.sort.mb* instellen op de gewenste waarden. Hier volgt een voorbeeld:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Deze parameter 4GB geheugen voor Java heap ruimte toegewezen en is ook sorteren efficiënter door meer geheugen toewijzen voor deze. Het is verstandig om te spelen met deze toewijzingen als er een taak fout fouten met betrekking tot het heap-ruimte.

2. **DFS blokgrootte** : deze parameter stelt de kleinste eenheid van de gegevens die in het bestandssysteem worden opgeslagen. Als u bijvoorbeeld als de blokgrootte DFS-128MB, vervolgens alle gegevens van de grootte kleiner dan en maximaal is 128MB opgeslagen in één blok, terwijl de gegevens die groter is dan 128MB extra blokken is toegewezen. Een zeer kleine blokgrootte kiezen ervoor zorgt dat grote overheadkosten in Hadoop aangezien het knooppunt met de naam heeft veel meer aanvragen om te zoeken naar het desbetreffende blok met betrekking tot het bestand te verwerken. Een aanbevolen instelling wanneer gigabyte betreft (of groter) gegevens zijn:

        set dfs.block.size=128m;

3. **Optimaliseren join-bewerking in component** : terwijl de join-bewerkingen in het kader van de kaart/verminderen meestal plaatsvinden in de verminderen fase soms enorme winsten kunnen worden bereikt door het plannen van joins in de fase van de kaart (ook wel 'mapjoins' genoemd). Rechtstreeks onderdeel om dit te doen indien mogelijk, kunt we instellen:

        set hive.auto.convert.join=true;

4. **Waarmee het aantal mappers aan component** : terwijl Hadoop kan de gebruiker het aantal verkleiningstoestellen, het aantal mappers is meestal niet door de gebruiker worden ingesteld. Een slag waarmee dat een zekere mate van controle op dit nummer is het kiezen van de variabelen Hadoop, *mapred.min.split.size* en *mapred.max.split.size* als de grootte van elke toegewezen taak wordt bepaald door:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    De standaardwaarde van *mapred.min.split.size* is meestal 0, die van *mapred.max.split.size* **Long.MAX** en die van *dfs.block.size* is 64 MB. Zoals u ziet, gegeven de gegevensgrootte van de afstemmen van deze parameters door "instelling" ze biedt ons de mogelijkheid om af te stemmen, het aantal mappers gebruikt.

5. Hieronder worden enkele andere meer **Geavanceerde opties** voor het optimaliseren van de prestaties van de component. Deze kunnen u het geheugen toegewezen aan toewijzen en taken verminderen en kunnen worden gebruikt voor het afstemmen van de prestaties. Houd er rekening mee dat de *mapreduce.reduce.memory.mb* kan niet groter dan de grootte van het fysieke geheugen van elke werknemer knooppunt in het cluster Hadoop zijn.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
