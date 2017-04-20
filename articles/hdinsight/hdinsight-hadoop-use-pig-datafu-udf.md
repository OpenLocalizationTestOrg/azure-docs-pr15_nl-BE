<properties
pageTitle="Gebruik DataFu met varkens op HDInsight"
description="DataFu is een verzameling van bibliotheken voor gebruik met Hadoop. Informatie over hoe u DataFu kunt gebruiken met varkens op het cluster HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Gebruik DataFu met varkens op HDInsight

DataFu is een verzameling van Open Source-bibliotheken voor gebruik met Hadoop. In dit document leert u hoe u DataFu op uw cluster HDInsight en DataFu gebruiker gedefinieerde functies (UDF) gebruiken met varkens.

##<a name="prerequisites"></a>Vereisten

* Een abonnement op Azure.

* Een cluster Azure HDInsight (Linux of Windows gebaseerd)

* Enigszins bekend bent met het [gebruik van varkens op HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>DataFu installeren op Linux-gebaseerde HDInsight

> [AZURE.NOTE] DataFu wordt geïnstalleerd op Linux gebaseerde clusters versie 3.3 of hoger, en op Windows gebaseerde clusters. Het is niet geïnstalleerd op Linux gebaseerde clusters ouder dan 3.3.
>
> Als u een Linux-gebaseerde cluster versie 3.3 of hoger, of een op Windows gebaseerde cluster gebruikt, kunt u deze sectie overslaan.

DataFu kunnen worden gedownload en geïnstalleerd vanaf de opslagplaats Maven. Gebruik de volgende stappen DataFu toevoegen aan het cluster HDInsight:

1. Verbinding maken met uw HDInsight op basis van Linux-cluster via SSH. Zie voor meer informatie over het gebruik van SSH in HDInsight, een van de volgende documenten:

    * [SSH gebruiken in combinatie met Hadoop op HDInsight van Linux, OS X en Unix, Linux gebaseerde](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Gebruik de volgende opdracht in het DataFu jar-bestand met het hulpprogramma wget downloaden of kopieer en plak de koppeling in uw browser om de download te starten.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Vervolgens het bestand uploaden naar de standaard opslag voor uw cluster HDInsight. Dit is het bestand beschikbaar op alle knooppunten in het cluster en het bestand blijft opgeslagen zelfs als u het verwijderen en opnieuw maken van het cluster.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] In het bovenstaande voorbeeld Slaat de jar in `wasbs:///example/jars` omdat deze map al op de clusteropslag bestaat. U kunt een locatie die u op de clusteropslag HDInsight wilt.

##<a name="use-datafu-with-pig"></a>DataFu met varkens gebruiken

De stappen in dit gedeelte wordt ervan uitgegaan dat u bekend zijn met het gebruik van varkens op HDInsight en bieden alleen de Latijnse varken overzichten niet de stappen op het gebruik ervan met het cluster. Zie voor meer informatie over het gebruik van varken met HDInsight [Varken met HDInsight gebruiken](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Wanneer u de DataFu van varken op een HDInsight op basis van Linux-cluster, moet u eerst het jar-bestand met de volgende instructie voor varkens Latijn registreren:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu is standaard op Windows gebaseerde HDInsight clusters geregistreerd.

Normaal gesproken definieert u een alias voor DataFu functies. Bijvoorbeeld:

    DEFINE SHA datafu.pig.hash.SHA();
    
Hiermee definieert u een alias met de naam `SHA` voor het SHA-hashing functie. Vervolgens kunt u dit in een varken Latijns schrift voor het genereren van een hash van de ingevoerde gegevens. Bijvoorbeeld vervangen het volgende door de namen in de ingevoerde gegevens een hash-waarde:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Als deze wordt gebruikt met de volgende gegevens invoeren:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
De volgende uitvoer wordt gegenereerd:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie op DataFu of varkens:

* [Handleiding voor Apache DataFu varkens](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
