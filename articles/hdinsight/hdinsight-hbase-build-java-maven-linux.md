<properties
    pageTitle="Een HBase-toepassing met behulp van Maven en Java bouwen en implementeren op Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over hoe een Apache HBase Java gebaseerde toepassing maakt, wordt deze implementeren in Linux-gebaseerde HDInsight in de Azure cloud met Apache Maven."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Maven gebruik te maken van Java-toepassingen die gebruikmaken van HBase met Linux-gebaseerde HDInsight (Hadoop)

Informatie over het maken en samenstellen van een [Apache HBase](http://hbase.apache.org/) -toepassing in Java met behulp van Apache Maven. Gebruik vervolgens de toepassing met een HDInsight op basis van Linux-cluster.

[Maven](http://maven.apache.org/) is een software projectmanagement en begrip hulpmiddel waarmee u software, documentatie en rapporten voor Java projecten bouwen. In dit artikel leert hoe u kunt gebruiken om een eenvoudige Java-toepassing maken die wordt gemaakt, query's, en een tabel HBase op een HDInsight op basis van Linux-cluster worden verwijderd.

> [AZURE.NOTE] De stappen in dit document wordt aangenomen dat u een HDInsight op basis van Linux-cluster. Zie voor meer informatie over het gebruik van een cluster met Windows-gebaseerde HDInsight [Maven gebruik te maken van Java-toepassingen die gebruikmaken van HBase met op Windows gebaseerde HDInsight](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Vereisten

* [Platform voor Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 of hoger

* [Maven](http://maven.apache.org/)

* [Een Linux-gebaseerde Azure HDInsight cluster met HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] De stappen in dit document zijn getest met HDInsight cluster versie 3.2, 3.3 en 3.4. De standaardwaarden die zijn opgegeven in de voorbeelden zijn voor een cluster HDInsight 3.4.

* **Vertrouwdheid met SSH en SCP**. Zie de volgende onderwerpen voor meer informatie over het gebruik van SSH en SCP met HDInsight:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Het project maken

1. Wijzigen vanaf de opdrachtregel in uw ontwikkelomgeving, mappen naar de locatie waar u wilt maken van het project, bijvoorbeeld `cd code/hdinsight`.

2. Gebruik de opdracht __mvn__ , die is geïnstalleerd met de Maven, voor het genereren van de steiger voor het project.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Dit maakt een nieuwe map in de huidige map met de naam die is opgegeven met de parameter __artifactID__ (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

    * __pom.XML__: het objectmodel van Project ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie voor het samenstellen van het project.

    * __bron__: de map waarin de map __main/java/com/microsoft/voorbeelden__ , waarin de toepassing wordt te ontwerpen.

3. Verwijder het bestand __src/test/java/com/microsoft/examples/apptest.java__ omdat het niet in dit voorbeeld worden gebruikt.

##<a name="update-the-project-object-model"></a>Het objectmodel van Project bijwerken

1. Bewerk het bestand __pom.xml__ en voeg de volgende code in de `<dependencies>` sectie:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Dit vertelt Maven dat het project __hbase client__ versie __1.1.2__vereist. Tijdens het compileren, dit gedownload uit de bibliotheek standaard Maven. U kunt de [Maven centrale opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) gebruiken voor meer informatie over deze afhankelijkheid.

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie van HBase die wordt geleverd met het cluster HDInsight. Gebruik de volgende tabel om te zoeken naar het juiste versienummer.

  	| Cluster-versie HDInsight | HBase versie gebruiken |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 en 3.4 | 1.1.2 |

    Zie [Wat zijn de verschillende Hadoop onderdelen beschikbaar met HDInsight](hdinsight-component-versioning.md)voor meer informatie over de versies van de HDInsight en -onderdelen.

2. Als u een HDInsight 3.3 of 3,4-cluster, moet u ook het volgende aan toevoegen de `<dependencies>` sectie:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Dit laadt de phoenix core-onderdelen die nodig zijn met versie Hbase 1.1.x.

2. De volgende code toevoegen aan het bestand __pom.xml__ . Dit moet binnen de `<project>...</project>` codes in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

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

    Hiermee wordt een bron (__hbase-OV-site.xml__), die configuratiegegevens voor HBase bevat.

    > [AZURE.NOTE] Ook kunt u configuratiewaarden via code instellen. Zie de opmerkingen in het voorbeeld van de __CreateTable__ die voor deze procedure volgt.

    Ook Hiermee wordt de [Maven-Compiler-Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) en [Maven grijs-invoegtoepassing](http://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compiler wordt gebruikt voor het compileren van de topologie. De tint van de invoegtoepassing wordt gebruikt om te voorkomen dat dubbele licentie in de JAR-pakket wordt gebouwd door Maven. De reden dat deze wordt gebruikt, is de dubbele licentiebestanden veroorzaakt een fout tijdens het uitvoeren van het cluster HDInsight. Grijs-maven-plugin met met de `ApacheLicenseResourceTransformer` uitvoering wordt voorkomen dat deze fout.

    Maven-grijs-plugin produceert ook een uber jar (of fat jar,) dat de afhankelijkheden die zijn vereist voor de toepassing bevat.

3. Sla het bestand __pom.xml__ .

4. Maak een nieuwe map met de naam __OV__ in de map __hbaseapp__ . Deze wordt gebruikt voor het opslaan van configuratiegegevens voor de verbinding met HBase.

5. Gebruik de volgende opdracht als u de configuratie HBase van de HDInsight-server kopiëren naar de map __OV__ . **Gebruikersnaam** vervangt het de naam van uw SSH-aanmelding. **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord op te geven. Als u een SSH-sleutel met de account gebruikt, moet u mogelijk gebruik van de `-i` het pad naar het bestand met de parameter. In het volgende voorbeeld wordt de persoonlijke sleutel van het geladen `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>De toepassing maken

1. Ga naar de map __hbaseapp/src/main/java/com/microsoft/voorbeelden__ en wijzig het bestand app.java in __CreateTable.java__.

2. Open het bestand __CreateTable.java__ en de bestaande inhoud te vervangen door het volgende:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. Maak een nieuw bestand met de naam __SearchByEmail.java__in de map __hbaseapp/src/main/java/com/microsoft/voorbeelden__ . Als de inhoud van dit bestand, gebruikt u het volgende:

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

6. Maak een nieuw bestand met de naam __DeleteTable.java__in de map __hbaseapp/src/main/hava/com/microsoft/voorbeelden__ . Als de inhoud van dit bestand, gebruikt u het volgende:

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

2. Gebruik de volgende opdracht maakt u een JAR-bestand met de toepassing vanuit de map __hbaseapp__ :

        mvn clean package

    Dit wist alle vorige build artefacten, downloads, eventuele afhankelijkheden die nog niet hebt geïnstalleerd, vervolgens bouwt en pakketten van de toepassing.

3. Als de opdracht is voltooid, bevat de map __hbaseapp/doel__ een bestand met de naam __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Het bestand __hbaseapp-1.0-SNAPSHOT.jar__ is een uber jar (ook wel een fat jar) waarin de afhankelijkheden die zijn vereist voor het uitvoeren van de toepassing.

##<a name="upload-the-jar-file-and-run-jobs"></a>Het JAR-bestand uploaden en uitvoeren van taken

1. Gebruik de volgende voor het uploaden van het oppervlak aan de cluster HDInsight. **Gebruikersnaam** vervangt het de naam van uw SSH-aanmelding. **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Dit zal het bestand uploaden naar de basismap voor uw gebruikersaccount SSH.

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord op te geven. Als u een SSH-sleutel met de account gebruikt, moet u mogelijk gebruik van de `-i` het pad naar het bestand met de parameter. In het volgende voorbeeld wordt de persoonlijke sleutel van het geladen `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Met SSH verbinding maken met het cluster HDInsight. **Gebruikersnaam** vervangt het de naam van uw SSH-aanmelding. **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Als u een wachtwoord voor uw account SSH gebruikt, wordt u gevraagd het wachtwoord op te geven. Als u een SSH-sleutel met de account gebruikt, moet u mogelijk gebruik van de `-i` het pad naar het bestand met de parameter. In het volgende voorbeeld wordt de persoonlijke sleutel van het geladen `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Zodra verbonden, gebruikt u de volgende om een nieuwe HBase tabel maken met de Java-toepassing:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Dit maakt een nieuwe HBase-tabel met de naam __mensen__, en gevuld met gegevens.

4. Gebruik vervolgens de volgende om te zoeken naar e-mailadressen die zijn opgeslagen in de tabel:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    U ontvangt de volgende resultaten:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>De tabel verwijderen

Wanneer u klaar bent met het voorbeeld, gebruikt u de volgende opdracht bij de Azure PowerShell-sessie verwijderen de __mensen__ tabel gebruikt in dit voorbeeld:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

