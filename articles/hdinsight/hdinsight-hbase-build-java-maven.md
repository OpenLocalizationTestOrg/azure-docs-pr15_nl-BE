<properties
pageTitle="Een Maven met HBase-toepassing bouwen en implementeren op Windows gebaseerde HDInsight | Microsoft Azure"
description="Informatie over het gebruik van Apache Maven op een Apache HBase Java gebaseerde toepassing maakt, wordt deze implementeren in een cluster op basis van Windows Azure HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Maven gebruik te maken van Java-toepassingen die gebruikmaken van HBase met op Windows gebaseerde HDInsight (Hadoop)

Informatie over het maken en samenstellen van een [Apache HBase](http://hbase.apache.org/) -toepassing in Java met behulp van Apache Maven. Gebruik vervolgens de toepassing met Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) is een software projectmanagement en begrip hulpmiddel waarmee u software, documentatie en rapporten voor Java projecten bouwen. Informatie over het maken van een eenvoudige Java-toepassing die worden gemaakt, query's, en verwijdert u een tabel HBase op een cluster Azure HDInsight in dit artikel.

> [AZURE.NOTE] De stappen in dit document wordt ervan uitgegaan dat u van een cluster met Windows-gebaseerde HDInsight gebruikmaakt. Zie voor meer informatie over het gebruik van een Linux-gebaseerde HDInsight cluster [Maven gebruik te maken van Java-toepassingen die gebruikmaken van HBase met Linux-gebaseerde HDInsight](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Vereisten

* [Platform voor Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger

* [Maven](http://maven.apache.org/)


* [Een cluster van Windows-gebaseerde HDInsight met HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] De stappen in dit document zijn getest met HDInsight cluster versie 3.2 en 3.3. De standaardwaarden die zijn opgegeven in de voorbeelden zijn voor een cluster HDInsight 3.3.

##<a name="create-the-project"></a>Het project maken

1. Wijzigen vanaf de opdrachtregel in uw ontwikkelomgeving mappen naar de locatie waar u wilt maken van het project, bijvoorbeeld `cd code\hdinsight`.

2. Gebruik de opdracht __mvn__ , die is geïnstalleerd met de Maven, voor het genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Met deze opdracht maakt een map in de huidige locatie met de naam die is opgegeven met de parameter __artifactID__ (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.XML__: het objectmodel van Project ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie voor het samenstellen van het project.

    * __bron__: de map waarin u de map __main\java\com\microsoft\examples__ , waarin de toepassing wordt te ontwerpen.

3. Verwijder het bestand __src\test\java\com\microsoft\examples\apptest.java__ omdat het niet in dit voorbeeld wordt gebruikt.

##<a name="update-the-project-object-model"></a>Het objectmodel van Project bijwerken

1. Bewerk het bestand __pom.xml__ en voeg de volgende code in de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    In deze sectie wordt Maven dat het project __hbase client__ versie __1.1.2__vereist. Tijdens het compileren, wordt deze afhankelijkheid gedownload uit de bibliotheek standaard Maven. U kunt de [Maven centrale opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) gebruiken voor meer informatie over deze afhankelijkheid.

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie van HBase die wordt geleverd met het cluster HDInsight. Gebruik de volgende tabel om te zoeken naar het juiste versienummer.

  	| Cluster-versie HDInsight | HBase versie gebruiken |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Zie [Wat zijn de verschillende Hadoop onderdelen beschikbaar met HDInsight](hdinsight-component-versioning.md)voor meer informatie over de versies van de HDInsight en -onderdelen.

2. Als u een cluster HDInsight 3.3, moet u ook het volgende aan toevoegen de `<dependencies>` sectie:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Deze afhankelijkheid wordt geladen, de phoenix core-onderdelen worden gebruikt door Hbase versie 1.1.x.

2. De volgende code toevoegen aan het bestand __pom.xml__ . Deze sectie moet deel uitmaken van de `<project>...</project>` codes in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
              </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                  </transformers>
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

    De `<resources>` sectie configureert u een bron met informatie over de configuratie voor HBase (__conf\hbase site.xml__).

    > [AZURE.NOTE] Ook kunt u configuratiewaarden via code instellen. Zie de opmerkingen in het voorbeeld van de __CreateTable__ die voor deze procedure volgt.

    Dit `<plugins>` sectie configureert u de [Maven-Compiler-Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) en [Maven grijs-invoegtoepassing](http://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compiler wordt gebruikt voor het compileren van de topologie. De tint van de invoegtoepassing wordt gebruikt om te voorkomen dat dubbele licentie in de JAR-pakket wordt gebouwd door Maven. De reden dat deze wordt gebruikt, is de dubbele licentiebestanden veroorzaakt een fout tijdens het uitvoeren van het cluster HDInsight. Grijs-maven-plugin met met de `ApacheLicenseResourceTransformer` uitvoering wordt voorkomen dat deze fout.

    Maven-grijs-plugin produceert ook een uber jar (of fat jar) die de afhankelijkheden die zijn vereist voor de toepassing bevat.

3. Sla het bestand __pom.xml__ .

4. Maak een nieuwe map met de naam __OV__ in de map __hbaseapp__ . Maak een bestand met de naam __hbase site.xml__in de directory __OV__ . Als de inhoud van het bestand, gebruikt u het volgende:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Dit bestand wordt gebruikt bij het laden de HBase-configuratie voor een cluster HDInsight.

    > [AZURE.NOTE] Dit is een minimale hbase-site.xml-bestand en bevat de absoluut minimale instellingen voor het cluster HDInsight.

3. Sla het bestand __hbase site.xml__ .

##<a name="create-the-application"></a>De toepassing maken

1. Ga naar de map __hbaseapp\src\main\java\com\microsoft\examples__ en wijzig het bestand app.java in __CreateTable.java__.

2. Open het bestand __CreateTable.java__ en de bestaande inhoud vervangen door de volgende code:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Dit is de __CreateTable__ klasse, wordt een tabel met de naam __mensen__ maken en deze vult met sommige vooraf gedefinieerde gebruikers.

3. Sla het bestand __CreateTable.java__ .

4. Maak een nieuw bestand met de naam __SearchByEmail.java__in de map __hbaseapp\src\main\java\com\microsoft\examples__ . De volgende code gebruiken als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    De klasse __SearchByEmail__ kan worden gebruikt om de query rijen op e-mailadres. Omdat het een reguliere expressie filter gebruikt, kunt u een tekenreeks of een reguliere expressie opgeven bij het gebruik van de klasse.

5. Sla het bestand __SearchByEmail.java__ .

6. Maak een nieuw bestand met de naam __DeleteTable.java__in de map __hbaseapp\src\main\hava\com\microsoft\examples__ . De volgende code gebruiken als de inhoud van dit bestand:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Deze klasse is in dit voorbeeld is opgeruimd en de tabel die is gemaakt door de klasse __CreateTable__ neer te zetten.

7. Sla het bestand __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Bouwen en de toepassing Inpakken

1. Open een opdrachtprompt en mappen verplaatsen naar de map __hbaseapp__ .

2. Gebruik de volgende opdracht maakt u een JAR-bestand met de toepassing:

        mvn clean package

    Dit wist alle vorige build artefacten, downloads, eventuele afhankelijkheden die nog niet hebt geïnstalleerd, vervolgens bouwt en pakketten van de toepassing.

3. Wanneer de opdracht is voltooid, bevat de map __hbaseapp\target__ een bestand met de naam __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Het bestand __hbaseapp-1.0-SNAPSHOT.jar__ is een uber jar (ook wel een fat jar) waarin de afhankelijkheden die zijn vereist voor het uitvoeren van de toepassing.

##<a name="upload-the-jar-file-and-start-a-job"></a>Het JAR-bestand uploaden en een taak starten

Er zijn veel manieren een bestand te uploaden naar uw cluster HDInsight zoals beschreven in het [uploaden van gegevens voor Hadoop projecten in HDInsight](hdinsight-upload-data.md). De volgende stappen gebruikt Azure PowerShell.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Na het installeren en configureren van Azure PowerShell, maak een nieuw bestand met de naam __hbase runner.psm1__. Als de inhoud van dit bestand, gebruikt u het volgende:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Dit bestand bevat twee modules:

    * __Toevoegen-HDInsightFile__ - gebruikt om bestanden te uploaden naar HDInsight

    * __Start HBaseExample__ - gebruikt voor het uitvoeren van de eerder gemaakte klassen

2. Sla het bestand __hbase runner.psm1__ .

3. Opent een nieuw venster met Azure PowerShell mappen wijzigen in de map __hbaseapp__ en voer de volgende opdracht.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Wijzig het pad naar de locatie van het bestand __hbase runner.psm1__ is eerder hebt gemaakt. Hiermee wordt de module voor deze Azure PowerShell-sessie.

2. Gebruik de volgende opdracht op de __hbaseapp-1.0-SNAPSHOT.jar__ uploaden naar uw cluster HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    __Hdinsightclustername__ vervangen door de naam van het cluster HDInsight. De __1.0-hbaseapp-SNAPSHOT.jar__ de opdracht geüpload naar de locatie van het __voorbeeld/potten__ in de primaire opslag voor uw cluster HDInsight.

3. Nadat de bestanden zijn geüpload, kunt u de volgende code gebruiken om een tabel maken met de __hbaseapp__:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    __Hdinsightclustername__ vervangen door de naam van het cluster HDInsight.

    Met deze opdracht maakt een nieuwe tabel met de __mensen__ in uw cluster HDInsight naam. Met deze opdracht wordt geen uitvoer niet weergegeven in het consolevenster.

2. Als u wilt zoeken naar posten in de tabel, gebruik de volgende opdracht:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    __Hdinsightclustername__ vervangen door de naam van het cluster HDInsight.

    Deze opdracht wordt de klasse **SearchByEmail** om te zoeken naar alle rijen waarin de familie __contactinformation__ kolom en de kolom __e-mailbericht__ bevat de tekenreeks __contoso.com__. U ontvangt de volgende resultaten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Met behulp van __fabrikam.com__ voor de `-emailRegex` geeft de gebruikers die in het veld e-mail __fabrikam.com__ waarde. Aangezien deze zoekopdracht met behulp van een reguliere expressie gebaseerde filter is geïmplementeerd, kunt u ook invoeren reguliere expressies, zoals __^ r__, waarbij het e-mailbericht met de letter "r begint" als resultaat gegeven gegevens.

##<a name="delete-the-table"></a>De tabel verwijderen

Wanneer u klaar bent met het voorbeeld, gebruikt u de volgende opdracht bij de Azure PowerShell-sessie verwijderen de __mensen__ tabel gebruikt in dit voorbeeld:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

__Hdinsightclustername__ vervangen door de naam van het cluster HDInsight.

##<a name="troubleshooting"></a>Het oplossen van problemen

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Geen resultaten of onverwachte resultaten wanneer u Start HBaseExample

Gebruik de `-showErr` -parameter voor de standaardfout (STDERR) dat wordt gegenereerd tijdens de uitvoering van de taak weergeven.
