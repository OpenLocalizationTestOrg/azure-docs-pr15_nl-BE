<properties
    pageTitle="Voorbeeld van gegevens in tabellen Azure HDInsight component | Microsoft Azure"
    description="Door gegevens in tabellen van Azure HDInsight (Hadopop) component bemonstering"
    services="machine-learning,hdinsight"
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

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Voorbeeldgegevens in tabellen Azure HDInsight component

We beschrijven hoe down steekproeven gegevens opgeslagen in Azure HDInsight component tabellen, query's component gebruiken in dit artikel. We hebben betrekking op drie popularly gebruikte bemonsteringsmethoden:

* Uniforme random sampling
* Steekproeven door groepen
* Toepassing van stratificatie bemonstering

**Waarom uw gegevens controleren?**
Als de gegevensset die u wilt analyseren groot is, is het meestal verstandig down-steekproef van de gegevens om deze naar een kleiner, maar representatieve en meer overzichtelijke omvang. Dit vereenvoudigt het wat gegevens, de exploratie en de functie engineering. Haar rol in het Team gegevens wetenschap proces is het snel maken van prototypen van de functies voor het verwerken van gegevens en computer modellen leren inschakelen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt beschreven hoe u de voorbeeldgegevens uit diverse storage-omgevingen.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak bemonstering is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Het indienen van query's component
Component-query's worden verzonden vanuit de console vanaf de opdrachtregel Hadoop op het hoofd knooppunt van het cluster Hadoop. Hiervoor log in het head-knooppunt van het cluster Hadoop, open de console vanaf de opdrachtregel Hadoop en query's van daaruit worden de component. Zie [component query's indienen](machine-learning-data-science-move-hive-tables.md#submit)voor instructies voor het indienen van query's component in de console vanaf de opdrachtregel Hadoop.

## <a name="uniform"></a>Uniforme random sampling
Uniforme steekproeven betekent dat elke rij in de gegevensset heeft een gelijke kans wordt bemonsterd. Dit kan worden geïmplementeerd door toe te voegen een extra veld ASELECT() de gegevensset in de binnenste query voor "select" en in de buitenste 'select'-query die voorwaarde op een willekeurig veld.

Hier volgt een van de voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Hier `<sample rate, 0-1>` geeft het deel van de records die de gebruikers als voorbeeld wilt nemen.

## <a name="group"></a>Steekproeven door groepen

Wanneer monsters bestaan gegevens, u kunt opnemen of uitsluiten van alle exemplaren van een bepaalde waarde van een variabele bestaan. Dit is wat wordt bedoeld met 'bemonstering per groep'.
Bijvoorbeeld als er een bestaan variabele "Staat", die heeft waarden NY, MA, CA, NJ, PA, enz, die records van dezelfde staat altijd samen, of zij of niet worden vastgelegd.

Hier volgt een van de voorbeeldquery die monsters per groep:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Toepassing van stratificatie bemonstering

Steekproeven is toepassing van stratificatie met betrekking tot een variabele bestaan wanneer de waarden van de verkregen monsters dat bestaan die zich in dezelfde verhouding als in de bovenliggende populatie waarvan de monsters zijn verkregen. Het voorbeeld hierboven, Stel dat uw gegevens subpopulaties door lidstaten, NJ heeft 100 waarnemingen, NY heeft 60 waarnemingen en WA 300 waarnemingen zeggen. Als u de frequentie van bemonstering toepassing stratificatie 0,5 opgeeft, vervolgens moet het monster verkregen ongeveer 50, 30 en 150 waarnemingen van NJ, NY en WA respectievelijk.

Hier volgt een van de voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Zie voor informatie over geavanceerde bemonsteringsmethoden die beschikbaar in de component zijn, [LanguageManual bemonstering](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
