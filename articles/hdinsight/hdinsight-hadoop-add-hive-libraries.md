<properties
pageTitle="Component bibliotheken toevoegen tijdens het maken van het cluster HDInsight | Azure"
description="Informatie over het onderdeel bibliotheken (jar-bestanden) toevoegen aan een cluster van HDInsight tijdens het maken van het cluster."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Component bibliotheken toevoegen tijdens het maken van het cluster HDInsight

Hebt u de bibliotheken die u vaak met het onderdeel op HDInsight gebruikt, wordt dit document bevat informatie over het gebruik van een scriptactie vooraf laden van de bibliotheken tijdens het maken van het cluster. Hiermee maakt u de bibliotheken wereldwijd beschikbaar in component (hoeft niet laden met [JAR toevoegen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) .)

##<a name="how-it-works"></a>Hoe het werkt

Wanneer u een cluster maakt, kunt u desgewenst een scriptactie waarmee een script wordt uitgevoerd op de clusterknooppunten terwijl ze worden gemaakt. Het script in dit document kan één parameter, een WASB locatie met de bibliotheken wordt (opgeslagen als jar-bestanden) die vooraf worden geladen.

Tijdens het maken van het cluster, het script worden de bestanden opgesomd, kopieert deze naar de `/usr/lib/customhivelibs/` map op de knooppunten van de kop en de werknemer, vervolgens toevoegt aan de `hive.aux.jars.path` eigenschap in het `core-site.xml` bestand. Op Linux gebaseerde clusters ook bijgewerkt het `hive-env.sh` -bestand met de locatie van de bestanden.

> [AZURE.NOTE] Met de scriptacties in dit artikel worden de bibliotheken beschikbaar in de volgende scenario's:
>
> * __HDInsight op basis van Linux__ - bij het gebruik van het __opdrachtregelprogramma component__ __WebHCat__ (Templeton) en __HiveServer2__.
> * __HDInsight op basis van Windows__ - bij het gebruik van het __opdrachtregelprogramma component__ en de __WebHCat__ (Templeton).

##<a name="the-script"></a>Het script

__Locatie van script__

Voor __Linux gebaseerde clusters__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Voor __Windows-gebaseerde clusters__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Vereisten__

* De scripts moeten worden toegepast op de __hoofd-knooppunten__ en de __knooppunten van de werknemer__.

* De potten die u wilt installeren, moeten in __dezelfde container__worden opgeslagen in Azure Blob-opslag. 

* De opslag rekening met de bibliotheek van de jar-bestanden __moet__ worden gekoppeld aan het cluster HDInsight tijdens het maken. Dit kan op twee manieren doen:

    * Door in een container op de standaardaccount voor de opslag voor het cluster.
    
    * Door in een container op een gekoppelde opslag container. Bijvoorbeeld in de portal kunt u __Optionele configuratie__ __gekoppelde opslag rekeningen__ extra opslagruimte.

* Het pad WASB naar de container moet worden opgegeven als een parameter voor de actie Script. Als de potten zijn opgeslagen in een container met de naam __libs__ op een opslag-account met de naam __mystorage__, de parameter zou bijvoorbeeld __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Dit document veronderstelt hebben al een account opslag maken, blob-container en de bestanden te uploaden. 
    >
    > Als u een opslag-account hebt gemaakt, kunt u dit doen via de [portal Azure](https://portal.azure.com). Vervolgens kunt u een hulpprogramma zoals [Opslagverkenner Azure](http://storageexplorer.com/) een nieuwe container in de account maken en bestanden te uploaden.

##<a name="create-a-cluster-using-the-script"></a>Maak een cluster met behulp van het script

> [AZURE.NOTE] De volgende stappen maakt een nieuw HDInsight op basis van Linux-cluster. Als u wilt een nieuwe Windows-gebaseerde clusters maken, __Windows__ selecteert als het cluster OS bij het maken van het cluster en (PowerShell) Windows script gebruiken in plaats van de bash-script.
> 
> Azure PowerShell of de HDInsight .NET SDK kunt u ook gebruiken voor het maken van een cluster met behulp van dit script. Zie voor meer informatie over het gebruik van deze methoden [clusters met scriptacties HDInsight aanpassen](hdinsight-hadoop-customize-cluster-linux.md).

1. Start een cluster wordt ingericht door de stappen in de [voorziening HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), maar Voer inrichten.

2. Op het blad **Optionele configuratie** **Scriptacties**selecteren en de gegevens verstrekken, zoals hieronder wordt weergegeven:

    * __Naam__: Geef een beschrijvende naam voor de scriptactie.
    * __SCRIPT-URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Kop__: Schakel dit selectievakje in
    * __Werknemer__: Schakel dit selectievakje in.
    * __ZOOKEEPER__: laat dit vak leeg.
    * __PARAMETERS__: Geef het adres van de WASB op de container- en rekening met de potten. Bijvoorbeeld __wasbs://libs@mystorage.blob.core.windows.net/__.

3. Gebruik de knop **Selecteer** de configuratie opslaan onderaan het **Scriptacties**.

4. Op de **Optionele configuratie** -blade __Gekoppelde opslag Accounts__ selecteren en selecteer de koppeling __toevoegen een sleutel voor de opslag__ . Selecteer de opslag rekening met de potten en gebruik de knoppen __Selecteer__ instellingen opslaan en de __Optionele configuratie__ blade.

5. Gebruik de knop **selecteren** op de onderkant van de **Optionele configuratie** blade optionele configuratie-informatie wilt opslaan.

6. Doorgaan met het cluster wordt ingericht als omschreven in [bepaling HDInsight clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Na het maken van het cluster is voltooid, is het mogelijk gebruik van de potten toegevoegd via dit script component zonder gebruik te maken van de `ADD JAR` instructie.

##<a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe component bibliotheken die zich in een cluster HDInsight jar-bestanden tijdens het maken van het cluster toevoegen. Zie voor meer informatie over het werken met component [Gebruik component met HDInsight](hdinsight-use-hive.md)
