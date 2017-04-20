<properties
    pageTitle="Gegevens in tabellen met query's component component verkennen | Microsoft Azure"
    description="Gegevens in het onderdeel tabellen met behulp van component query's verkennen."
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Gegevens in tabellen component met component query's verkennen

Dit document bevat voorbeeldscripts component waarmee gegevens in de tabellen van de component in een cluster HDInsight Hadoop verkennen.

De volgende **menu** koppelingen naar onderwerpen over het gebruik van extra gegevens uit diverse storage-omgevingen te verkennen.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u beschikt over:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, ziet u [een opslag Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Een aangepaste Hadoop cluster met de service HDInsight ingericht. Als u instructies, Zie [Aanpassen Azure HDInsight Hadoop Clusters voor geavanceerde Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* De gegevens is geüpload naar de tabellen van de component in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, de instructies in [component tabellen maken en laden gegevens](machine-learning-data-science-move-hive-tables.md) eerst gegevens uploaden naar tabellen component.
* Externe toegang tot het cluster ingeschakeld. Zie [toegang tot het hoofd van Hadoop Cluster knooppunt](machine-learning-data-science-customize-hadoop-cluster.md#headnode)als u instructies nodig hebt.
* Als u instructies voor het verzenden van query's component nodig, Zie [component query's verzenden](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Voorbeeld van de component query scripts voor verkennen

1. Het aantal waarnemingen per partitie `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Het aantal waarnemingen per dag `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Ophalen van de niveaus in een kolom bestaan  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Het aantal niveaus in een combinatie van twee bestaan kolommen ophalen `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. De verdeling voor numerieke kolommen ophalen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Ophalen van records uit twee tabellen koppelen

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Scripts voor scenario's taxi rit gegevens extra query

Voorbeelden van query's die specifiek voor [RDAM Taxi rit gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn zijn ook beschikbaar in [de bibliotheek Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebben gegevensschema opgegeven en klaar om te worden ingediend om uit te voeren.
