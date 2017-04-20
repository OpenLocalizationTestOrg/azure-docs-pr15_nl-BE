<properties
pageTitle="Een Java door de gebruiker gedefinieerde functie (UDF) gebruiken met de component in HDInsight | Microsoft Azure"
description="Informatie over het maken en gebruiken van een Java door de gebruiker gedefinieerde functie (UDF) van de component in HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Gebruik een Java UDF met de component in HDInsight

Component is ideaal voor het werken met gegevens in de HDInsight, maar soms moet u een meer algemeen doel van de taal. Component kunt u maken door de gebruiker gedefinieerde functies (UDF) met behulp van een aantal verschillende programmeertalen. In dit document leert u het gebruik van een Java-UDF uit component.

## <a name="requirements"></a>Vereisten

* Een abonnement op Azure

* Een cluster van HDInsight (Windows of Linux-gebaseerde)

    > [AZURE.NOTE] De meeste stappen in dit document zullen werken op beide clustertypen; de stappen voor het uploaden van de gecompileerde UDF aan het cluster en uit te voeren zijn echter specifiek voor Linux gebaseerde clusters. Koppelingen worden naar informatie die kan worden gebruikt met Windows-gebaseerde clusters geleverd.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 of hoger (of een gelijkwaardige, zoals OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Een teksteditor of Java IDE

    > [AZURE.IMPORTANT] Als u een server Linux-gebaseerde HDInsight, maar de Python bestanden maken op een Windows-client, moet u een editor die gebruikmaakt van LF als een lijn-uitgang. Als u niet zeker weet of uw editor LF of CRLF gebruikt, Zie de sectie [problemen oplossen](#troubleshooting) voor instructies over het gebruik van hulpprogramma's op het cluster HDInsight CR-teken wordt verwijderd.

## <a name="create-an-example-udf"></a>Een voorbeeld van de UDF maken

1. Gebruik de volgende een nieuwe Maven-project te maken vanaf een opdrachtregel:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Als u PowerShell gebruikt, moet u aanhalingstekens rond de parameters plaatsen. Bijvoorbeeld `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Hiermee maakt u een nieuwe map met de naam __exampleudf__, waarin het Maven-project.

2. Zodra het project is gemaakt, verwijdert u de map __src-exampleudf/test__ die is gemaakt als onderdeel van het project. niet wordt gebruikt in dit voorbeeld.

3. Open het __exampleudf/pom.xml__en het bestaande `<dependencies>` post met het volgende:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Deze vermeldingen geven de versie van Hadoop en component opgenomen met HDInsight 3.3 en 3.4 clusters. Hier vindt u informatie over de versies van Hadoop en HDInsight voorzien van het document [HDInsight onderdeel versioning](hdinsight-component-versioning.md) component.

    Voeg toe een `<build>` sectie voordat u de `</project>` regel aan het einde van het bestand. In deze sectie, moet het volgende bevatten:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Deze vermeldingen definiëren het bouwen van het project. Met name de versie van Java die gebruikmaakt van het project en het bouwen van een uberjar voor de implementatie van het cluster.

    Sla het bestand op nadat de wijzigingen zijn aangebracht.

4. Wijzig __exampleudf/src/main/java/com/microsoft/examples/App.java__ in __ExampleUDF.java__en open het bestand in uw editor.

5. De inhoud van het bestand __ExampleUDF.java__ vervangen door de volgende, en vervolgens het bestand opslaat.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Dit implementeert een UDF accepteert een string-waarde en een kleine versie van de tekenreeks als resultaat gegeven.

## <a name="build-and-install-the-udf"></a>Compileren en installeren van de UDF

1. Gebruik de volgende opdracht om te compileren en de UDF pakket:

        mvn compile package

    Dit zal maken en vervolgens de UDF inpakken in __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Gebruik de `scp` opdracht het bestand te kopiëren naar het cluster HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    __Myuser__ vervangen door de SSH-gebruikersaccount voor de cluster. __Mijncluster__ vervangen door de naam van het cluster. Als u een wachtwoord gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord op te geven. Als u een certificaat gebruikt, moet u mogelijk gebruik van de `-i` parameter voor het opgeven van het bestand met de persoonlijke sleutel.

3. Verbinding maken met het cluster via SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Zie de volgende documenten voor meer informatie over het gebruik van SSH in HDInsight.

    * [SSH gebruiken met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Uit de SSH-sessie het jar-bestand te kopiëren naar de opslag van HDInsight.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>De UDF uit component gebruiken

1. Gebruik de volgende de Beeline-client van de SSH-sessie starten.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Met deze opdracht wordt aangenomen dat de standaardinstelling van de __beheerder__ voor de aanmeldingsaccount voor uw cluster.

2. Zodra u aankomt bij de `jdbc:hive2://localhost:10001/>` wordt gevraagd, voert u het volgende om de UDF naar component toevoegen en deze weer te geven als een functie.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. De UDF wordt gebruikt om waarden uit een tabel opgehaald om tekenreeksen in kleine letters.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Dit zal het apparaatplatform (Android, Windows, iOS, enz.) te selecteren in de tabel, de tekenreeks converteren naar kleine letters en weer te geven. De uitvoer ziet er ongeveer als volgt.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Volgende stappen

Zie voor andere manieren om te werken met de component [Gebruiken component met HDInsight](hdinsight-use-hive.md).

Voor meer informatie over functies voor Hive User-Defined, Zie de [component operatoren en functies van de door de gebruiker gedefinieerde](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) sectie van de component wiki op apache.org.
