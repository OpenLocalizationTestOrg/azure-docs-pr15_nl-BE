<properties
   pageTitle="Analyseren en proces JSON-documenten met de component in HDInsight | Microsoft Azure"
   description="Informatie over het JSON-documenten gebruiken en analyseren ze met behulp van de component in HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Verwerken en analyseren van JSON-documenten met behulp van de component in HDInsight

Informatie over het verwerken en analyseren van JSON bestanden met behulp van de component in HDInsight. De volgende JSON-document wordt in de zelfstudie gebruikt

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Het bestand kan worden gevonden op wasbs://processjson@hditutorialdata.blob.core.windows.net/. Zie voor meer informatie over het gebruik van Azure Blob-opslag met HDInsight [Gebruik HDFS-compatibele Azure Blob-opslag met Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Kopieer het bestand naar de standaardcontainer van het cluster als u wilt.

In deze zelfstudie gebruikt u de component-console.  Zie voor meer informatie van het openen van de console van de component [Gebruiken component met Hadoop op HDInsight met extern bureaublad](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>JSON-documenten samenvoegen

De methoden die in de volgende sectie vereist de JSON-document in één rij. U moet dus de JSON-document naar een tekenreeks afvlakken. Als het JSON-document al afgevlakt is, kunt u deze stap overslaan en ga direct naar de volgende sectie JSON analyseren van gegevens.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

De raw JSON-bestand bevindt zich op **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. De tabel *StudentsRaw* component verwijst naar het onbewerkte, niet-afgevlakte JSON-document.

De tabel *StudentsOneLine* component zal de gegevens opslaan in het bestandssysteem van HDInsight standaard het pad */json/studenten /* .

De INSERT-instructie de tabel StudentOneLine met de afgevlakte JSON-gegevens invullen.

De SELECT-instructie slechts terug 1 rij.

Hier ziet u de uitvoer van de SELECT-instructie:

![Afvlakking van de JSON-document.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>JSON-documenten in de component te analyseren

Component biedt drie verschillende mechanismen voor het uitvoeren van query's op de JSON-documenten:

- Gebruik de GET\_JSON\_OBJECT UDF (door gebruiker gedefinieerde functie)
- het JSON_TUPLE UDF gebruiken
- aangepaste SerDe gebruiken
- Schrijf dat u UDF Python of andere talen met de eigenaar. [In dit artikel] Zie[ hdinsight-python] voor het uitvoeren van Python code met de component.

### <a name="use-the-getjsonobject-udf"></a>Gebruik de GET\_JSON_OBJECT UDF
Component bevat een ingebouwde UDF [json-object ophalen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) dat JSON opvragen tijdens runtime kunt uitvoeren. Deze methode heeft twee argumenten: de naam en de naam van de methode die de afgevlakte JSON-document en de JSON-veld dat moet worden geparseerd. We bekijken een voorbeeld om te zien hoe deze UDF werkt.

Ophalen van de voornaam en achternaam voor elke student

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Hier is de output wanneer deze query wordt uitgevoerd in het consolevenster.

![UDF-get_json_object][image-hdi-hivejson-getjsonobject]

Er zijn enkele beperkingen van de UDF get-json_object.

- Omdat voor elk veld in de query de query opnieuw te parseren, heeft dit gevolgen voor de prestaties.
- GET\_JSON_OBJECT() retourneert de tekenreeks van een matrix. Om dit converteren naar een matrix component, hebt u de vierkante haken vervangen met reguliere expressies ' [' en ']' en vervolgens ook gesplitst als u de matrix.


Dit is de reden waarom de wiki onderdeel raadt het gebruik van json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Het JSON_TUPLE UDF gebruiken

Een andere UDF geleverd door de component wordt [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) beter dan [get_ json-_object presteert](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)genoemd. Deze methode wordt een set sleutels en een JSON-string en geeft als resultaat een tupel van waarden met een functie. De volgende query geeft als resultaat de student-id en de kwaliteit van de JSON-document:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

De uitvoer van dit script in de console van de component:

![UDF-json_tuple][image-hdi-hivejson-jsontuple]

JSON\_TUPEL maakt gebruik van de syntaxis van de [zijdelingse weergave](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) in Component waarmee json\_tupel een virtuele tabel maken door de functie UDT toepassen op elke rij van de oorspronkelijke tabel.  Complexe JSONs zijn door het herhaalde gebruik van de zijdelingse weergave te onhandig. JSON_TUPLE kan niet verder verwerken geneste JSONs.


###<a name="use-custom-serde"></a>Aangepaste SerDe gebruiken

SerDe is de beste keuze voor het parseren van geneste JSON-documenten, kunt u het schema gebruiken om de documenten te parseren en de JSON-schema definiëren. In deze zelfstudie gebruikt u een van de meer populaire SerDe die is ontwikkeld door [rcongiu](https://github.com/rcongiu).

**De aangepaste SerDe gebruiken:**

1. Installeer [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Kies de Windows X64-versie van de JDK als u wilt gebruikmaken van de Windows-implementatie van HDInsight

    >[AZURE.WARNING] JDK 1,8 werkt niet met deze SerDe.

    Nadat de installatie is voltooid, voegt u een nieuwe omgevingsvariabele toe:

    1. Open **View Geavanceerde systeeminstellingen** van het scherm van Windows.
    2. Klik op **omgevingsvariabelen**.  
    3. Voeg dat een nieuwe **JAVA_HOME** omgevingsvariabele verwijst naar **C:\Program Files\Java\jdk1.7.0_55** of waar de JDK is geïnstalleerd.

    ![Instellen van de juiste configuratie waarden voor JDK][image-hdi-hivejson-jdk]

2. [3.3.1 Maven](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip) installeren

    De map bin naar uw pad toevoegen door te gaan naar het besturingselement Configuratiescherm--> de variabelen voor de account Environment variabelen bewerken. Het screenshot hieronder toont u hoe u dit doet.

    ![Maven instellen][image-hdi-hivejson-maven]

3. Het project van [Component-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site klonen. U kunt dit doen door te klikken op de knop "Download Zip", zoals in de screenshot hieronder.

    ![Klonen van het project][image-hdi-hivejson-serde]

4: Ga naar de map waarin u dit pakket en type "mvn" hebt gedownload. Dit moet u vervolgens naar het cluster kopiëren kunt nodig jar-bestanden maken.

5: Ga naar het pad van de map onder de hoofdmap waarin u het pakket hebt gedownload. Het json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar bestand uploaden naar de hoofd-knooppunt van het cluster. Ik meestal plaatsen onder de component binary map: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin of iets dergelijks.

6: Typ in het dialoogvenster component "jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar toevoegen". Aangezien in mijn geval, het oppervlak in de map C:\apps\dist\hive-0.13.x\bin is, kan ik direct het oppervlak met de naam toevoegen, zoals hieronder wordt weergegeven:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

U bent nu gereed voor gebruik van de SerDe bij query's uitvoeren tegen de JSON-document.

De volgende instructie een tabel maken met een gedefinieerd schema

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Overzicht van de voornaam en achternaam van de leerling

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Hier is het resultaat van de console van de component.

![Query SerDe 1][image-hdi-hivejson-serde_query1]

Voor het berekenen van de som van de scores van de JSON-document

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

De bovenstaande query gebruikt [zijdelingse weergave exploderen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF de array van scores uitbreiden, zodat ze kunnen worden opgeteld.

Hier wordt de uitvoer op de console van de component.

![Query SerDe 2][image-hdi-hivejson-serde_query2]

Selecteer welke onderwerpen een bepaalde student is meer dan 80 punten gescoord zoeken  
      JT. StudentClassCollection.ClassId van json_table jt zijdelingse weergave exploderen (jt. De collectie StudentClassCollection.Score) als score waar score > 80;

De bovenstaande query als resultaat een matrix van de component in tegenstelling tot get\_json\_-object dat een tekenreeks als resultaat gegeven.

![Query SerDe 3][image-hdi-hivejson-serde_query3]

Als u skil wilt verkeerd JSON, en vervolgens zoals beschreven in de [wiki-pagina](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) van deze SerDe u die bereiken kunt door de volgende code:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Samenvatting
Kortom, het type JSON-operator in de component die u kiest afhankelijk is van uw scenario. Als u een eenvoudig JSON-document hebt en u slechts één veld hoeft om te zoeken op – kunt u gebruik van de component UDF get\_json\_object. Als er meer dan één sleutels om te zoeken op kunt u json_tuple gebruiken. Als u een geneste document hebt, moet u de JSON-SerDe gebruiken.

Zie voor andere verwante artikelen

- [Component- en HiveQL met Hadoop in HDInsight gebruiken voor het analyseren van een voorbeeldbestand van Apache log4j](hdinsight-use-hive.md)
- [Vlucht vertraging gegevens analyseren met behulp van de component in HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Twitter analyseren met behulp van de component in HDInsight](hdinsight-analyze-twitter-data.md)
- [Een Hadoop-up met behulp van DocumentDB en HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
